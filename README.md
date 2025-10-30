using System;
using System.Drawing;
using System.Windows.Forms;
using System.Numerics;

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
            this.ClientSize = new Size(400, 200);
            this.StartPosition = FormStartPosition.CenterScreen;
            this.Font = new Font("Arial", 10);
            this.Padding = new Padding(10);
            
            // Заголовок
            this.lblTitle = new Label();
            this.lblTitle.Location = new Point(20, 20);
            this.lblTitle.Size = new Size(350, 25);
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
            this.lblResult.Size = new Size(350, 80);
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
                    // Вычисляем 2^n с помощью BigInteger для работы с большими числами
                    BigInteger result = BigInteger.Pow(2, n);
                    lblResult.Text = $"2^{n} = {result:N0}";
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







using System;
using System.Drawing;
using System.Windows.Forms;

namespace HundredsCounter
{
    public class Form1 : Form
    {
        private TextBox txtInput;
        private Button btnCalculate;
        private Label lblResult;
        private Label lblPrompt;

        public Form1()
        {
            InitializeComponent();
        }

        private void InitializeComponent()
        {
            this.txtInput = new TextBox();
            this.btnCalculate = new Button();
            this.lblResult = new Label();
            this.lblPrompt = new Label();
            
            // Настройка формы
            this.SuspendLayout();
            this.Text = "Счетчик сотен";
            this.ClientSize = new Size(234, 161);
            this.StartPosition = FormStartPosition.CenterScreen;
            
            // txtInput
            this.txtInput.Location = new Point(15, 35);
            this.txtInput.Size = new Size(200, 20);
            
            // btnCalculate
            this.btnCalculate.Location = new Point(15, 70);
            this.btnCalculate.Size = new Size(200, 30);
            this.btnCalculate.Text = "Определить число сотен";
            this.btnCalculate.Click += new EventHandler(this.btnCalculate_Click);
            
            // lblResult
            this.lblResult.AutoSize = true;
            this.lblResult.Font = new Font("Microsoft Sans Serif", 10F);
            this.lblResult.Location = new Point(12, 110);
            this.lblResult.Text = "Результат тут";
            
            // lblPrompt
            this.lblPrompt.AutoSize = true;
            this.lblPrompt.Location = new Point(12, 15);
            this.lblPrompt.Text = "Введите натуральное число (>99):";
            
            // Добавление контролов на форму
            this.Controls.AddRange(new Control[] {
                this.txtInput, this.btnCalculate, this.lblResult, this.lblPrompt
            });
            
            this.ResumeLayout(false);
        }

        private void btnCalculate_Click(object sender, EventArgs e)
        {
            if (int.TryParse(txtInput.Text, out int number))
            {
                if (number > 99)
                {
                    int hundreds = (number / 100) % 10;
                    lblResult.Text = $"Число сотен: {hundreds}";
                }
                else
                {
                    MessageBox.Show("Число должно быть больше 99!", "Ошибка", 
                                MessageBoxButtons.OK, MessageBoxIcon.Warning);
                }
            }
            else
            {
                MessageBox.Show("Введите корректное натуральное число!", "Ошибка", 
                            MessageBoxButtons.OK, MessageBoxIcon.Error);
            }
        }

        [STAThread]
        static void Main()
        {
            Application.EnableVisualStyles();
            Application.SetCompatibleTextRenderingDefault(false);
            Application.Run(new Form1());
        }
    }
}