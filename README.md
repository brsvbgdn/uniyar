using System;
using System.Windows.Forms;

namespace EquilateralTriangleArea
{
    public partial class MainForm : Form
    {
        private TextBox sideTextBox;
        private Button calculateButton;
        private Label resultLabel;

        public MainForm()
        {
            InitializeComponent();
        }

        private void InitializeComponent()
        {
            this.sideTextBox = new TextBox();
            this.calculateButton = new Button();
            this.resultLabel = new Label();
            
            // Настройка формы
            this.Text = "Площадь равностороннего треугольника";
            this.ClientSize = new System.Drawing.Size(300, 150);
            this.StartPosition = FormStartPosition.CenterScreen;
            
            // Настройка поля ввода
            this.sideTextBox.Location = new System.Drawing.Point(20, 20);
            this.sideTextBox.Size = new System.Drawing.Size(100, 20);
            this.sideTextBox.PlaceholderText = "Длина стороны";
            
            // Настройка кнопки
            this.calculateButton.Location = new System.Drawing.Point(130, 18);
            this.calculateButton.Size = new System.Drawing.Size(150, 25);
            this.calculateButton.Text = "Вычислить площадь";
            this.calculateButton.Click += new EventHandler(this.CalculateButton_Click);
            
            // Настройка метки результата
            this.resultLabel.Location = new System.Drawing.Point(20, 60);
            this.resultLabel.Size = new System.Drawing.Size(250, 30);
            this.resultLabel.Text = "Результат: ";
            
            // Добавление элементов на форму
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