using System;
using System.Windows.Forms;

namespace WindowsFormsApp1
{
    public partial class Form1 : Form
    {
        public Form1()
        {
            InitializeComponent();
        }

        // Функция f(x)
        double f(double x)
        {
            // Здесь можешь подставить нужную формулу
            // Например: f(x) = sin(x) + cos(x)
            return Math.Sin(x) + Math.Cos(x);
        }

        private void button1_Click(object sender, EventArgs e)
        {
            try
            {
                // Получение исходных данных
                double x = Convert.ToDouble(textBox1.Text);
                double y = Convert.ToDouble(textBox2.Text);
                double z = Convert.ToDouble(textBox3.Text); // z вводится, но не участвует в формуле (если нужно – можно добавить)

                double s;

                // Вычисление по условиям
                if (1 < x * y && x * y < 10)
                {
                    s = Math.Exp(f(x));
                }
                else if (12 < x * y && x * y < 40)
                {
                    s = Math.Sqrt(f(x) + 4 * y);
                }
                else
                {
                    s = y * Math.Pow(f(x), 2);
                }

                // Вывод результата
                textBox4.Text = "Результаты работы программы студента Петрова И.И." + Environment.NewLine;
                textBox4.Text += $"При X = {x}" + Environment.NewLine;
                textBox4.Text += $"При Y = {y}" + Environment.NewLine;
                textBox4.Text += $"При Z = {z}" + Environment.NewLine;
                textBox4.Text += $"S = {s:F4}" + Environment.NewLine;
            }
            catch (Exception ex)
            {
                MessageBox.Show("Ошибка ввода данных: " + ex.Message);
            }
        }
    }
}
