using System;
using System.Drawing;
using System.Windows.Forms;

namespace ExpressionCalculator
{
    public class Form1 : Form
    {
        private Button btnCalculate;
        private TextBox txtX, txtY, txtZ, txtResult;

        public Form1()
        {
            this.Size = new Size(400, 300);
            this.Text = "Expression Calculator";

            // Создаем элементы управления
            CreateControls();
        }

        private void CreateControls()
        {
            // Поле X
            new Label { Text = "X:", Location = new Point(20, 20), Parent = this };
            txtX = new TextBox { Location = new Point(100, 20), Width = 200, Text = "-4,5", Parent = this };

            // Поле Y
            new Label { Text = "Y:", Location = new Point(20, 50), Parent = this };
            txtY = new TextBox { Location = new Point(100, 50), Width = 200, Text = "0,000075", Parent = this };

            // Поле Z
            new Label { Text = "Z:", Location = new Point(20, 80), Parent = this };
            txtZ = new TextBox { Location = new Point(100, 80), Width = 200, Text = "84,5", Parent = this };

            // Результат
            new Label { Text = "Result:", Location = new Point(20, 110), Parent = this };
            txtResult = new TextBox { Location = new Point(100, 110), Width = 200, ReadOnly = true, Parent = this };

            // Кнопка
            btnCalculate = new Button { Text = "Calculate", Location = new Point(100, 150), Width = 100, Parent = this };
            btnCalculate.Click += (s, e) => Calculate();
        }

        private void Calculate()
        {
            try
            {
                double x = double.Parse(txtX.Text.Replace(".", ","));
                double y = double.Parse(txtY.Text.Replace(".", ","));
                double z = double.Parse(txtZ.Text.Replace(".", ","));

                double absDiff = Math.Abs(x - y);
                double firstPart = Math.Pow(8 + Math.Pow(absDiff, 2) + 1, 1.0 / 3.0);
                double tanZSquaredPlusOne = Math.Pow(Math.Tan(z), 2) + 1;
                double secondPart = Math.Exp(absDiff) * Math.Pow(tanZSquaredPlusOne, x);
                double result = firstPart - secondPart;

                txtResult.Text = result.ToString("F6");
            }
            catch (Exception ex)
            {
                MessageBox.Show("Error: " + ex.Message);
            }
        }

        [STAThread]
        static void Main()
        {
            Application.EnableVisualStyles();
            Application.Run(new Form1());
        }
    }
}