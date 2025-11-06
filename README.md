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
            this.Size = new Size(800, 600);
            this.Text = "Graph of Function";
            this.Paint += new PaintEventHandler(Form1_Paint);
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
                double y = 1e-3 * Math.Pow(Math.Abs(x), 2.5) + Math.Log(Math.Abs(x + b));
                points.Add(new PointF((float)x, (float)y));
            }

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

            // Создаем матрицу преобразования
            RectangleF graphArea = new RectangleF(minX, minY, maxX - minX, maxY - minY);
            RectangleF displayArea = new RectangleF(50, 50, this.ClientSize.Width - 100, this.ClientSize.Height - 100);

            // Рисуем оси координат
            DrawAxes(e.Graphics, graphArea, displayArea);
            
            // Рисуем график
            using (Pen pen = new Pen(Color.Blue, 2))
            {
                for (int i = 0; i < points.Count - 1; i++)
                {
                    PointF p1 = TransformPoint(points[i], graphArea, displayArea);
                    PointF p2 = TransformPoint(points[i + 1], graphArea, displayArea);
                    e.Graphics.DrawLine(pen, p1, p2);
                }
            }
        }

        private PointF TransformPoint(PointF point, RectangleF graphArea, RectangleF displayArea)
        {
            float x = displayArea.Left + (point.X - graphArea.Left) * displayArea.Width / graphArea.Width;
            float y = displayArea.Bottom - (point.Y - graphArea.Bottom) * displayArea.Height / graphArea.Height;
            return new PointF(x, y);
        }

        private void DrawAxes(Graphics g, RectangleF graphArea, RectangleF displayArea)
        {
            using (Pen axisPen = new Pen(Color.Black, 1))
            {
                // Ось Y
                PointF yStart = TransformPoint(new PointF(0, graphArea.Top), graphArea, displayArea);
                PointF yEnd = TransformPoint(new PointF(0, graphArea.Bottom), graphArea, displayArea);
                g.DrawLine(axisPen, yStart, yEnd);

                // Ось X
                PointF xStart = TransformPoint(new PointF(graphArea.Left, 0), graphArea, displayArea);
                PointF xEnd = TransformPoint(new PointF(graphArea.Right, 0), graphArea, displayArea);
                g.DrawLine(axisPen, xStart, xEnd);
            }
        }
    }
}