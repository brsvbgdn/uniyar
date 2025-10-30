using System;
using System.Drawing;
using System.Windows.Forms;

namespace RectangleDraw
{
    public class MainForm : Form
    {
        public MainForm()
        {
            InitializeComponent();
        }

        private void InitializeComponent()
        {
            this.SuspendLayout();
            // 
            // MainForm
            // 
            this.AutoScaleDimensions = new SizeF(6F, 13F);
            this.AutoScaleMode = AutoScaleMode.Font;
            this.ClientSize = new Size(300, 250);
            this.Name = "MainForm";
            this.Text = "Прямоугольник";
            this.Paint += new PaintEventHandler(this.MainForm_Paint);
            this.ResumeLayout(false);
        }

        private void MainForm_Paint(object sender, PaintEventArgs e)
        {
            using (Pen pen = new Pen(Color.Blue, 2))
            {
                Point[] points =
                {
                    new Point(80, 80),
                    new Point(170, 80),
                    new Point(170, 150),
                    new Point(80, 150)
                };
                e.Graphics.DrawPolygon(pen, points);
            }
        }

        [STAThread]
        static void Main()
        {
            Application.EnableVisualStyles();
            Application.SetCompatibleTextRenderingDefault(false);
            Application.Run(new MainForm());
        }
    }
}