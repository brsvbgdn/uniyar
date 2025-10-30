using System;
using System.Drawing;
using System.Windows.Forms;

namespace NumberTransformation
{
    public partial class MainForm : Form
    {
        private TextBox txtX;
        private TextBox txtY;
        private TextBox txtZ;
        private Button btnCalculate;
        private Label lblResult;
        private Label lblX;
        private Label lblY;
        private Label lblZ;

        public MainForm()
        {
            InitializeForm();
        }

        private void InitializeForm()
        {
            // Настройка формы
            this.Text = "Преобразование чисел";
            this.ClientSize = new Size(400, 250);
            this.StartPosition = FormStartPosition.CenterScreen;
            this.Font = new Font("Arial", 10);
            
            // Метка и поле для X
            this.lblX = new Label();
            this.lblX.Location = new Point(20, 20);
            this.lblX.Size = new Size(100, 20);
            this.lblX.Text = "Число X:";
            
            this.txtX = new TextBox();
            this.txtX.Location = new Point(120, 20);
            this.txtX.Size = new Size(100, 20);
            
            // Метка и поле для Y
            this.lblY = new Label();
            this.lblY.Location = new Point(20, 50);
            this.lblY.Size = new Size(100, 20);
            this.lblY.Text = "Число Y:";
            
            this.txtY = new TextBox();
            this.txtY.Location = new Point(120, 50);
            this.txtY.Size = new Size(100, 20);
            
            // Метка и поле для Z
            this.lblZ = new Label();
            this.lblZ.Location = new Point(20, 80);
            this.lblZ.Size = new Size(100, 20);
            this.lblZ.Text = "Число Z:";
            
            this.txtZ = new TextBox();
            this.txtZ.Location = new Point(120, 80);
            this.txtZ.Size = new Size(100, 20);
            
            // Кнопка вычисления
            this.btnCalculate = new Button();
            this.btnCalculate.Location = new Point(20, 120);
            this.btnCalculate.Size = new Size(200, 30);
            this.btnCalculate.Text = "Выполнить преобразование";
            this.btnCalculate.Click += new EventHandler(this.CalculateButton_Click);
            
            // Метка результата
            this.lblResult = new Label();
            this.lblResult.Location = new Point(20, 170);
            this.lblResult.Size = new Size(350, 60);
            this.lblResult.Text = "Результат: ";
            
            // Добавление элементов на форму
            this.Controls.Add(this.lblX);
            this.Controls.Add(this.txtX);
            this.Controls.Add(this.lblY);
            this.Controls.Add(this.txtY);
            this.Controls.Add(this.lblZ);
            this.Controls.Add(this.txtZ);
            this.Controls.Add(this.btnCalculate);
            this.Controls.Add(this.lblResult);
        }

        private void CalculateButton_Click(object sender, EventArgs e)
        {
            if (double.TryParse(txtX.Text, out double x) && 
                double.TryParse(txtY.Text, out double y) && 
                double.TryParse(txtZ.Text, out double z))
            {
                // Проверяем, что числа попарно различные
                if (x == y  x == z  y == z)
                {
                    lblResult.Text = "Ошибка: числа должны быть попарно различными";
                    return;
                }

                double newX = x, newY = y, newZ = z;
                
                if (x + y + z < 1)
                {
                    // Если сумма меньше 1, находим наименьшее из трех чисел
                    double min = Math.Min(x, Math.Min(y, z));
                    
                    if (min == x)
                        newX = (y + z) / 2;else if (min == y)
                        newY = (x + z) / 2;
                    else
                        newZ = (x + y) / 2;
                    
                    lblResult.Text = $"Сумма чисел < 1.\nНаименьшее число заменено полусуммой двух других.\nРезультат: X = {newX:F2}, Y = {newY:F2}, Z = {newZ:F2}";
                }
                else
                {
                    // Если сумма >= 1, находим меньшее из X и Y
                    if (x < y)
                    {
                        newX = (y + z) / 2;
                        lblResult.Text = $"Сумма чисел >= 1.\nМеньшее из X и Y (X) заменено полусуммой двух других.\nРезультат: X = {newX:F2}, Y = {newY:F2}, Z = {newZ:F2}";
                    }
                    else
                    {
                        newY = (x + z) / 2;
                        lblResult.Text = $"Сумма чисел >= 1.\nМеньшее из X и Y (Y) заменено полусуммой двух других.\nРезультат: X = {newX:F2}, Y = {newY:F2}, Z = {newZ:F2}";
                    }
                }
            }
            else
            {
                lblResult.Text = "Ошибка: введите корректные числа";
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


