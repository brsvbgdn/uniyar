using System;
using System.Drawing;
using System.Windows.Forms;
using System.Linq;

namespace ProductCalculator
{
    public class MainForm : Form
    {
        private TextBox txtNumbers;
        private Button btnCalculate;
        private Label lblResult;
        private Label lblPrompt;
        private Button btnClear;

        public MainForm()
        {
            InitializeComponent();
        }

        private void InitializeComponent()
        {
            // Настройка формы
            this.SuspendLayout();
            this.Text = "Вычисление произведения чисел";
            this.ClientSize = new Size(300, 220);
            this.StartPosition = FormStartPosition.CenterScreen;
            this.Font = new Font("Microsoft Sans Serif", 8.25F);

            // Поле ввода чисел
            this.txtNumbers = new TextBox();
            this.txtNumbers.Location = new Point(15, 35);
            this.txtNumbers.Multiline = true;
            this.txtNumbers.Size = new Size(270, 80);
            this.txtNumbers.TabIndex = 0;

            // Кнопка вычисления
            this.btnCalculate = new Button();
            this.btnCalculate.Location = new Point(15, 130);
            this.btnCalculate.Size = new Size(120, 30);
            this.btnCalculate.Text = "Вычислить произведение";
            this.btnCalculate.Click += new EventHandler(this.btnCalculate_Click);

            // Кнопка очистки
            this.btnClear = new Button();
            this.btnClear.Location = new Point(145, 130);
            this.btnClear.Size = new Size(120, 30);
            this.btnClear.Text = "Очистить";
            this.btnClear.Click += new EventHandler(this.btnClear_Click);

            // Метка результата
            this.lblResult = new Label();
            this.lblResult.AutoSize = true;
            this.lblResult.Font = new Font("Microsoft Sans Serif", 10F);
            this.lblResult.Location = new Point(12, 170);
            this.lblResult.Text = "Результат";
            this.lblResult.Size = new Size(200, 30);

            // Метка подсказки
            this.lblPrompt = new Label();
            this.lblPrompt.AutoSize = true;
            this.lblPrompt.Location = new Point(12, 15);
            this.lblPrompt.Text = "Введите числа через пробел или запятую (a1 a2 ... an):";
            this.lblPrompt.Size = new Size(270, 13);

            // Добавление элементов на форму
            this.Controls.AddRange(new Control[] {
                this.txtNumbers, this.btnCalculate, this.btnClear,
                this.lblResult, this.lblPrompt
            });

            this.ResumeLayout(false);
        }

        private void btnCalculate_Click(object sender, EventArgs e)
        {
            try
            {
                // Получаем числа из текстового поля
                string input = txtNumbers.Text;
                
                // Разбиваем строку на числа, разделенные пробелами/запятыми
                string[] numberStrings = input.Split(new char[] { ' ', ',', ';', '\t' }, 
                                                    StringSplitOptions.RemoveEmptyEntries);
                
                // Преобразуем строки в действительные числа
                double[] numbers = numberStrings.Select(s => double.Parse(s)).ToArray();
                
                // Проверяем, что n > 0
                if (numbers.Length == 0)
                {
                    MessageBox.Show("Введите хотя бы одно число!", "Ошибка", 
                                    MessageBoxButtons.OK, MessageBoxIcon.Warning);
                    return;
                }

                // Вычисляем произведение
                double product = 1.0;
                foreach (double number in numbers)
                {
                    product *= number;
                }

                // Выводим результат
                lblResult.Text = $"Произведение чисел: {product:F6}";
            }
            catch (FormatException)
            {
                MessageBox.Show("Ошибка в формате чисел! Убедитесь, что введены только числа.", 
                                "Ошибка", MessageBoxButtons.OK, MessageBoxIcon.Error);
            }
            catch (OverflowException)
            {
                MessageBox.Show("Слишком большое или маленькое число!", 
                                "Ошибка", MessageBoxButtons.OK, MessageBoxIcon.Error);
            }
            catch (Exception ex)
            {
                MessageBox.Show($"Произошла ошибка: {ex.Message}", 
                                "Ошибка", MessageBoxButtons.OK, MessageBoxIcon.Error);
            }
        }

        private void btnClear_Click(object sender, EventArgs e)
        {
            txtNumbers.Clear();
            lblResult.Text = "Результат";
        }

        [STAThread]
        static void Main()
        {
            Application.EnableVisualStyles();
            Application.SetCompatibleTextRenderingDefault(false);
            Application.Run(new MainForm());
        }
    }
}