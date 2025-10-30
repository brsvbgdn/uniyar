 using System;
using System.Drawing;
using System.Windows.Forms;

namespace EquilateralTriangleArea
{
    public partial class MainForm : Form
    {
        private TextBox txtSide;
        private Button btnCalculate;
        private Label lblResult;
        private Label lblPrompt;

        public MainForm()
        {
            SetupForm();
        }

        private void SetupForm()
        {
            // Настройка формы
            this.Text = "Площадь равностороннего треугольника";
            this.ClientSize = new Size(300, 150);
            this.StartPosition = FormStartPosition.CenterScreen;
            
            // Метка-подсказка
            this.lblPrompt = new Label();
            this.lblPrompt.Location = new Point(20, 20);
            this.lblPrompt.Size = new Size(100, 15);
            this.lblPrompt.Text = "Длина стороны:";
            
            // Поле ввода
            this.txtSide = new TextBox();
            this.txtSide.Location = new Point(20, 40);
            this.txtSide.Size = new Size(100, 20);
            
            // Кнопка вычисления
            this.btnCalculate = new Button();
            this.btnCalculate.Location = new Point(130, 38);
            this.btnCalculate.Size = new Size(150, 25);
            this.btnCalculate.Text = "Вычислить площадь";
            this.btnCalculate.Click += new EventHandler(this.CalculateButton_Click);
            
            // Метка результата
            this.lblResult = new Label();
            this.lblResult.Location = new Point(20, 80);
            this.lblResult.Size = new Size(250, 30);
            this.lblResult.Text = "Результат: ";
            
            // Добавление элементов на форму
            this.Controls.Add(this.lblPrompt);
            this.Controls.Add(this.txtSide);
            this.Controls.Add(this.btnCalculate);
            this.Controls.Add(this.lblResult);
        }

        private void CalculateButton_Click(object sender, EventArgs e)
        {
            if (double.TryParse(txtSide.Text, out double side) && side > 0)
            {
                double area = (Math.Sqrt(3) / 4) * Math.Pow(side, 2);
                lblResult.Text = $"Площадь: {area:F2}";
            }
            else
            {
                lblResult.Text = "Ошибка: введите положительное число";
            }
        }
    }

    public static class Program
    {
        [STAThread]
        public static void Main()
        {
            Application.EnableVisualStyles();
            Application.SetCompatibleTextRenderingDefault(false);
            Application.Run(new MainForm());
        }
    }
}