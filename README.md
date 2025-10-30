using System;
using System.Drawing;
using System.Windows.Forms;
using System.Text;

namespace PowerOfTwoCalculator
{
    public partial class MainForm : Form
    {
        private TextBox txtNumber;
        private Button btnCalculate;
        private Label lblResult;
        private Label lblTitle;

        public MainForm()
        {
            InitializeForm();
        }

        private void InitializeForm()
        {
            // Настройка формы
            this.Text = "Вычисление 2^n";
            this.ClientSize = new Size(500, 250);
            this.StartPosition = FormStartPosition.CenterScreen;
            this.Font = new Font("Arial", 10);
            this.Padding = new Padding(10);
            
            // Заголовок
            this.lblTitle = new Label();
            this.lblTitle.Location = new Point(20, 20);
            this.lblTitle.Size = new Size(450, 25);
            this.lblTitle.Text = "Введите натуральное число n:";
            this.lblTitle.Font = new Font("Arial", 11, FontStyle.Bold);
            
            // Поле для ввода числа
            this.txtNumber = new TextBox();
            this.txtNumber.Location = new Point(20, 50);
            this.txtNumber.Size = new Size(150, 25);
            this.txtNumber.KeyPress += TxtNumber_KeyPress;
            
            // Кнопка вычисления
            this.btnCalculate = new Button();
            this.btnCalculate.Location = new Point(180, 48);
            this.btnCalculate.Size = new Size(150, 30);
            this.btnCalculate.Text = "Вычислить 2^n";
            this.btnCalculate.Click += new EventHandler(this.CalculateButton_Click);
            
            // Метка результата
            this.lblResult = new Label();
            this.lblResult.Location = new Point(20, 100);
            this.lblResult.Size = new Size(450, 120);
            this.lblResult.Text = "Результат: ";
            this.lblResult.BorderStyle = BorderStyle.FixedSingle;
            this.lblResult.BackColor = Color.LightYellow;
            
            // Добавление элементов на форму
            this.Controls.Add(this.lblTitle);
            this.Controls.Add(this.txtNumber);
            this.Controls.Add(this.btnCalculate);
            this.Controls.Add(this.lblResult);
        }

        // Обработчик для проверки ввода (только цифры)
        private void TxtNumber_KeyPress(object sender, KeyPressEventArgs e)
        {
            if (!char.IsControl(e.KeyChar) && !char.IsDigit(e.KeyChar))
            {
                e.Handled = true;
            }
        }

        // Функция для умножения числа в строковой форме на 2
        private string MultiplyByTwo(string number)
        {
            StringBuilder result = new StringBuilder();
            int carry = 0;
            
            // Проходим по цифрам числа справа налево
            for (int i = number.Length - 1; i >= 0; i--)
            {
                int digit = int.Parse(number[i].ToString());
                int product = digit * 2 + carry;
                result.Insert(0, (product % 10).ToString());
                carry = product / 10;
            }
            
            // Если есть остаток, добавляем его в начало
            if (carry > 0)
            {
                result.Insert(0, carry.ToString());
            }
            
            return result.ToString();
        }

        private void CalculateButton_Click(object sender, EventArgs e)
        {
            if (string.IsNullOrWhiteSpace(txtNumber.Text))
            {
                lblResult.Text = "Ошибка: введите число n";
                return;
            }

            if (int.TryParse(txtNumber.Text, out int n) && n >= 0)
            {
                try
                {
                    string result;
                    
                    if (n == 0)
{
                        result = "1"; // 2^0 = 1
                    }
                    else
                    {
                        // Начинаем с 1 и умножаем на 2 n раз
                        result = "1";
                        for (int i = 0; i < n; i++)
                        {
                            result = MultiplyByTwo(result);
                        }
                    }
                    
                    lblResult.Text = $"2^{n} = {result}";
                }
                catch (Exception ex)
                {
                    lblResult.Text = $"Ошибка вычисления: {ex.Message}";
                }
            }
            else
            {
                lblResult.Text = "Ошибка: введите натуральное число (n ≥ 0)";
            }
        }
    }

    internal static class Program
    {
        [STAThread]
        private static void Main()
        {
            Application.EnableVisualStyles();
            Application.SetCompatibleTextRenderingDefault(false);
            Application.Run(new MainForm());
        }
    }
}
