using System;
using System.Drawing;
using System.Windows.Forms;

namespace EquilateralTriangleArea
{
    public partial class MainForm : Form
    {
        private TextBox sideTextBox;
        private Button calculateButton;
        private Label resultLabel;
        private Label promptLabel;

        public MainForm()
        {
            InitializeComponent();
        }

        private void InitializeComponent()
        {
            // Настройка формы
            this.Text = "Площадь равностороннего треугольника";
            this.ClientSize = new Size(300, 150);
            this.StartPosition = FormStartPosition.CenterScreen;
            
            // Метка-подсказка
            this.promptLabel = new Label();
            this.promptLabel.Location = new Point(20, 20);
            this.promptLabel.Size = new Size(100, 15);
            this.promptLabel.Text = "Длина стороны:";
            
            // Поле ввода
            this.sideTextBox = new TextBox();
            this.sideTextBox.Location = new Point(20, 40);
            this.sideTextBox.Size = new Size(100, 20);
            
            // Кнопка вычисления
            this.calculateButton = new Button();
            this.calculateButton.Location = new Point(130, 38);
            this.calculateButton.Size = new Size(150, 25);
            this.calculateButton.Text = "Вычислить площадь";
            this.calculateButton.Click += new EventHandler(this.CalculateButton_Click);
            
            // Метка результата
            this.resultLabel = new Label();
            this.resultLabel.Location = new Point(20, 80);
            this.resultLabel.Size = new Size(250, 30);
            this.resultLabel.Text = "Результат: ";
            
            // Добавление элементов на форму
            this.Controls.Add(this.promptLabel);
            this.Controls.Add(this.sideTextBox);
            this.Controls.Add(this.calculateButton);
            this.Controls.Add(this.resultLabel);
        }

        private void CalculateButton_Click(object sender, EventArgs e)
        {
            if (double.TryParse(sideTextBox.Text, out double side) && side > 0)
            {
                double area = (Math.Sqrt(3) / 4) * Math.Pow(side, 2);
                resultLabel.Text = $"Площадь: {area:F2}";
            }
            else
            {
                resultLabel.Text = "Ошибка: введите положительное число";
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