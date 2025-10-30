using System;
using System.Drawing;
using System.Windows.Forms;

namespace RectangleDraw
{
    public partial class Form1 : Form
    {
        public Form1()
        {
            InitializeComponent();
        }

        private void Form1_Paint(object sender, PaintEventArgs e)
        {
            // Создаем перо для рисования (синий цвет, толщина 2px)
            using (Pen pen = new Pen(Color.Blue, 2))
            {
                // Рисуем прямоугольник по заданным вершинам
                Point[] points =
                {
                    new Point(80, 80),   // Верхний левый
                    new Point(170, 80),  // Верхний правый
                    new Point(170, 150), // Нижний правый
                    new Point(80, 150)   // Нижний левый
                };

                // Соединяем точки линиями
                e.Graphics.DrawPolygon(pen, points);
            }
        }
    }
}