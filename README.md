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