using System;
using System.Collections.Generic;
using System.Drawing;
using System.Windows.Forms;

namespace FunctionGraph
{
    public partial class Form1 : Form
    {
        public Form1()
        {
            InitializeComponent();
        }

        private void InitializeComponent()
        {
            this.SuspendLayout();
            this.AutoScaleDimensions = new SizeF(6F, 13F);
            this.AutoScaleMode = AutoScaleMode.Font;
            this.ClientSize = new Size(800, 600);
            this.Name = "Form1";
            this.Text = "Graph of Function y = 10^-3 * |x|^(5/2) + ln|x+35.4|";
            this.Paint += new PaintEventHandler(this.Form1_Paint);
            this.ResumeLayout(false);
        }

        private void Form1_Paint(object sender, PaintEventArgs e)
        {
            double x0 = 1.75;
            double xk = -2.5;
            double dx = -0.25;
            double b = 35.4;

            List<PointF> points = new List<PointF>();
            
            // Вычисляем точки графика
            for (double x = x0; x >= xk; x += dx)
            {
                try
                {
                    // y = 10^-3 * |x|^(5/2) + ln|x+b|
                    double y = 1e-3 * Math.Pow(Math.Abs(x), 2.5) + Math.Log(Math.Abs(x + b));
                    points.Add(new PointF((float)x, (float)y));
                }
                catch (Exception ex)
                {
                    Console.WriteLine($"Error at x={x}: {ex.Message}");
                }
            }

            if (points.Count == 0) return;

            // Находим минимальные и максимальные значения
            float minX = float.MaxValue;
            float maxX = float.MinValue;
            float minY = float.MaxValue;
            float maxY = float.MinValue;

            foreach (PointF p in points)
            {
                if (p.X < minX) minX = p.X;
                if (p.X > maxX) maxX = p.X;
                if (p.Y < minY) minY = p.Y;
                if (p.Y > maxY) maxY = p.Y;
            }

            // Добавляем отступы
            float xRange = maxX - minX;
            float yRange = maxY - minY;
            minX -= xRange * 0.1f;
            maxX += xRange * 0.1f;
            minY -= yRange * 0.1f;
            maxY += yRange * 0.1f;

            // Если диапазон по Y слишком мал, увеличиваем его
            if (yRange < 0.1f)
            {
                minY -= 0.5f;
                maxY += 0.5f;
            }

            // Область данных
            RectangleF graphArea = new RectangleF(minX, minY, maxX - minX, maxY - minY);
            // Область отображения на экране
            RectangleF displayArea = new RectangleF(50, 50, this.ClientSize.Width - 100, this.ClientSize.Height - 100);

            // Очищаем фон
            e.Graphics.Clear(Color.White);
            
            // Рисуем сетку и оси
            DrawGridAndAxes(e.Graphics, graphArea, displayArea);
            
            // Рисуем график
            DrawGraph(e.Graphics, points, graphArea, displayArea);
            
            // Подписываем график
            DrawLabels(e.Graphics, graphArea, displayArea);
        }

        private void DrawGridAndAxes(Graphics g, RectangleF graphArea, RectangleF displayArea)
        {
            using (Pen gridPen = new Pen(Color.LightGray, 1))
            using (Pen axisPen = new Pen(Color.Black, 2))
            using (Font font = new Font("Arial", 8))
            using (Brush textBrush = new SolidBrush(Color.Black))
            {
                // Вертикальные линии сетки (по X)
                float xStep = (graphArea.Width) / 10;
                for (float x = graphArea.Left; x <= graphArea.Right; x += xStep)
                {
                    PointF p1 = TransformPoint(new PointF(x, graphArea.Top), graphArea, displayArea);
                    PointF p2 = TransformPoint(new PointF(x, graphArea.Bottom), graphArea, displayArea);
                    g.DrawLine(gridPen, p1, p2);
                    
                    // Подпись по X
                    PointF labelPos = TransformPoint(new PointF(x, graphArea.Bottom), graphArea, displayArea);
                    g.DrawString(x.ToString("F2"), font, textBrush, labelPos.X - 10, labelPos.Y + 5);
                }

                // Горизонтальные линии сетки (по Y)
                float yStep = graphArea.Height / 10;
                for (float y = graphArea.Top; y <= graphArea.Bottom; y += yStep)
                {
                    PointF p1 = TransformPoint(new PointF(graphArea.Left, y), graphArea, displayArea);
                    PointF p2 = TransformPoint(new PointF(graphArea.Right, y), graphArea, displayArea);
                    g.DrawLine(gridPen, p1, p2);
                    
                    // Подпись по Y
                    PointF labelPos = TransformPoint(new PointF(graphArea.Left, y), graphArea, displayArea);
                    g.DrawString(y.ToString("F2"), font, textBrush, labelPos.X - 35, labelPos.Y - 8);
                }

                // Ось Y
                if (graphArea.Left <= 0 && graphArea.Right >= 0)
                {
                    PointF yStart = TransformPoint(new PointF(0, graphArea.Top), graphArea, displayArea);
                    PointF yEnd = TransformPoint(new PointF(0, graphArea.Bottom), graphArea, displayArea);
                    g.DrawLine(axisPen, yStart, yEnd);
                }

                // Ось X
                if (graphArea.Top <= 0 && graphArea.Bottom >= 0)
                {
                    PointF xStart = TransformPoint(new PointF(graphArea.Left, 0), graphArea, displayArea);
                    PointF xEnd = TransformPoint(new PointF(graphArea.Right, 0), graphArea, displayArea);
                    g.DrawLine(axisPen, xStart, xEnd);
                }
            }
        }

        private void DrawGraph(Graphics g, List<PointF> points, RectangleF graphArea, RectangleF displayArea)
        {
            using (Pen graphPen = new Pen(Color.Red, 2))
            {
                for (int i = 0; i < points.Count - 1; i++)
                {
                    PointF p1 = TransformPoint(points[i], graphArea, displayArea);
                    PointF p2 = TransformPoint(points[i + 1], graphArea, displayArea);
                    g.DrawLine(graphPen, p1, p2);
                }

                // Рисуем точки
                using (Brush pointBrush = new SolidBrush(Color.Blue))
                {
                    foreach (PointF point in points)
                    {
                        PointF screenPoint = TransformPoint(point, graphArea, displayArea);
                        g.FillEllipse(pointBrush, screenPoint.X - 3, screenPoint.Y - 3, 6, 6);
                    }
                }
            }
        }

        private void DrawLabels(Graphics g, RectangleF graphArea, RectangleF displayArea)
        {
            using (Font font = new Font("Arial", 10))
            using (Brush brush = new SolidBrush(Color.Black))
            {
                g.DrawString("X", font, brush, this.ClientSize.Width - 20, displayArea.Bottom + 10);
                g.DrawString("Y", font, brush, displayArea.Left - 30, 10);
                
                // Заголовок графика
                using (Font titleFont = new Font("Arial", 12, FontStyle.Bold))
                {
                    g.DrawString("y = 10⁻³ * |x|^(5/2) + ln|x+35.4|", titleFont, brush, 
                                displayArea.Left, 10);
                }
                
                // Информация о диапазоне
                g.DrawString($"X: [{graphArea.Left:F2}, {graphArea.Right:F2}]", font, brush, 
                            displayArea.Left, displayArea.Bottom + 20);
                g.DrawString($"Y: [{graphArea.Top:F2}, {graphArea.Bottom:F2}]", font, brush, 
                            displayArea.Left, displayArea.Bottom + 40);
            }
        }

        private PointF TransformPoint(PointF point, RectangleF graphArea, RectangleF displayArea)
        {
            float x = displayArea.Left + (point.X - graphArea.Left) * displayArea.Width / graphArea.Width;
            float y = displayArea.Bottom - (point.Y - graphArea.Bottom) * displayArea.Height / graphArea.Height;
            return new PointF(x, y);
        }

        [STAThread]
        static void Main()
        {
            Application.EnableVisualStyles();
            Application.SetCompatibleTextRenderingDefault(false);
            Application.Run(new Form1());
        }
    }
}