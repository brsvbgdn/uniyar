using System;
using System.Windows.Forms;

namespace MyFirstApp
{
    public partial class Form1 : Form
    {
        public Form1()
        {
            InitializeComponent();
        }

        private void Form1_Load(object sender, EventArgs e)
        {
            // Начальное значение X
            textBox1.Text = "-4,5";
            // Начальное значение Y
            textBox2.Text = "0,000075";
            // Начальное значение Z
            textBox3.Text = "84,5";
        }

        private void button1_Click(object sender, EventArgs e)
        {
            // Считывание значения X
            double x = double.Parse(textBox1.Text);
            // Вывод значения X в окно
            textBox4.Text += Environment.NewLine +
            "X = " + x.ToString();
            
            // Считывание значения Y
            double y = double.Parse(textBox2.Text);
            // Вывод значения Y в окно
            textBox4.Text += Environment.NewLine +
            "Y = " + y.ToString();
            
            // Считывание значения Z
            double z = double.Parse(textBox3.Text);
            // Вывод значения Z в окно
            textBox4.Text += Environment.NewLine +
            "Z = " + z.ToString();
            
            // Вычисляем арифметическое выражение
            // |x - y|
            double absXY = Math.Abs(x - y);
            
            // Первая часть: ∛(8 + |x - y|² + 1)
            double firstPart = Math.Pow(8 + Math.Pow(absXY, 2) + 1, 1.0 / 3.0);
            
            // Вторая часть: e^{|x - y|} * (tg²z + 1)^x
            double tgZ = Math.Tan(z);
            double tgZSquaredPlusOne = Math.Pow(tgZ, 2) + 1;
            double secondPart = Math.Exp(absXY) * Math.Pow(tgZSquaredPlusOne, x);
            
            // Итоговый результат
            double u = firstPart - secondPart;
            
            // Выводим результат в окно
            textBox4.Text += Environment.NewLine +
            "Результат U = " + u.ToString("F6");
            
            // Добавляем разделитель для следующего вычисления
            textBox4.Text += Environment.NewLine +
            "---------------------------" + Environment.NewLine;
        }
    }
}