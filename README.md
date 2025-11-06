using System;
using System.Collections.Generic;
using System.Drawing;
using System.Windows.Forms;

namespace SimpleFunctionGraph
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
            this.ClientSize = new System.Drawing.Size(1000, 700);
            this.Text = "Graph of Function";
            this.Paint += new PaintEventHandler(this.Form1_Paint);
            this.ResumeLayout(false);
        }

        private void Form1_Paint(object sender, PaintEventArgs e)
        {
            e.Graphics.Clear(Color.White);
            
            double x0 = 1.75;
            double xk = -2.5;
            double dx = -0.25;
            double b = 35.4;

            List<PointF> points = new List<PointF>();
            
            // Вычисляем точки
            for (double x = x0; x >= xk; x += dx)
            {
                try
                {
                    double y = 1e-3 * Math.Pow(Math.Abs(x), 2.5) + Math.Log(Math.Abs(x + b));
                    points.Add(new PointF((float)x, (float)y));
                }
                catch { }
            }

            if (points.Count == 0) return;

            // Рисуем оси
            DrawAxes(e.Graphics);
            
            // Рисуем график
            using (Pen pen = new Pen(Color.Red, 2))
            {
                for (int i = 0; i < points.Count - 1; i++)
                {
                    PointF p1 = ScalePoint(points[i]);
                    PointF p2 = ScalePoint(points[i + 1]);
                    e.Graphics.DrawLine(pen, p1, p2);
                }
            }

            // Подписи
            using (Font font = new Font("Arial", 10))
            using (Brush brush = new SolidBrush(Color.Black))
            {
                e.Graphics.DrawString("y = 10⁻³ * |x|^(5/2) + ln|x+35.4|", 
                    new Font("Arial", 12, FontStyle.Bold), brush, 20, 20);
            }
        }

        private void DrawAxes(Graphics g)
        {
            int centerX = this.ClientSize.Width / 2;
            int centerY = this.ClientSize.Height / 2;
            float scale = 50; // Масштаб

            using (Pen axisPen = new Pen(Color.Black, 2))
            using (Pen gridPen = new Pen(Color.LightGray, 1))
            {
                // Ось X
                g.DrawLine(axisPen, 0, centerY, this.ClientSize.Width, centerY);
                // Ось Y
                g.DrawLine(axisPen, centerX, 0, centerX, this.ClientSize.Height);

                // Сетка
                for (int x = -10; x <= 10; x++)
                {
                    int screenX = centerX + (int)(x * scale);
                    g.DrawLine(gridPen, screenX, 0, screenX, this.ClientSize.Height);
                }
                for (int y = -10; y <= 10; y++)
                {
                    int screenY = centerY + (int)(y * scale);
                    g.DrawLine(gridPen, 0, screenY, this.ClientSize.Width, screenY);
                }
            }
        }

        private PointF ScalePoint(PointF point)
        {
            int centerX = this.ClientSize.Width / 2;
            int centerY = this.ClientSize.Height / 2;
            float scale = 50; // Масштаб

            return new PointF(
                centerX + point.X * scale,
                centerY - point.Y * scale
            );
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