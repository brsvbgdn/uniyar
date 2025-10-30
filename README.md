using System;
using System.Windows.Forms;

namespace HundredsCounter
{
    public partial class MainForm : Form
    {
        public MainForm()
        {
            InitializeComponent();
        }

        private void btnCalculate_Click(object sender, EventArgs e)
        {
            if (int.TryParse(txtInput.Text, out int number))
            {
                if (number > 99)
                {
                    int hundreds = number / 100;
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
    }
}