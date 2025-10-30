using System;
using System.Drawing;
using System.Windows.Forms;

namespace NumberTransformationApp
{
    public class TransformationForm : Form
    {
        private TextBox txtX, txtY, txtZ;
        private Button btnCalculate;
        private Label lblResult, lblX, lblY, lblZ;

        public TransformationForm()
        {
            BuildForm();
        }

        private void BuildForm()
        {
            // Настройка формы
            Text = "Преобразование чисел";
            Size = new Size(450, 300);
            StartPosition = FormStartPosition.CenterScreen;
            Font = new Font("Arial", 10);
            Padding = new Padding(10);
            
            // Создаем таблицу для размещения элементов
            TableLayoutPanel mainTable = new TableLayoutPanel();
            mainTable.Dock = DockStyle.Fill;
            mainTable.ColumnCount = 2;
            mainTable.RowCount = 5;
            mainTable.ColumnStyles.Add(new ColumnStyle(SizeType.Absolute, 100));
            mainTable.ColumnStyles.Add(new ColumnStyle(SizeType.Percent, 100));
            
            // Метка и поле для X
            lblX = new Label { Text = "Число X:", AutoSize = true, Anchor = AnchorStyles.Left };
            txtX = new TextBox { Anchor = AnchorStyles.Left | AnchorStyles.Right };
            
            // Метка и поле для Y
            lblY = new Label { Text = "Число Y:", AutoSize = true, Anchor = AnchorStyles.Left };
            txtY = new TextBox { Anchor = AnchorStyles.Left | AnchorStyles.Right };
            
            // Метка и поле для Z
            lblZ = new Label { Text = "Число Z:", AutoSize = true, Anchor = AnchorStyles.Left };
            txtZ = new TextBox { Anchor = AnchorStyles.Left | AnchorStyles.Right };
            
            // Кнопка вычисления (занимает две колонки)
            btnCalculate = new Button 
            { 
                Text = "Выполнить преобразование",
                Anchor = AnchorStyles.Left | AnchorStyles.Right,
                Height = 35
            };
            btnCalculate.Click += CalculateButton_Click;
            
            // Метка результата (занимает две колонки)
            lblResult = new Label 
            { 
                Text = "Результат: ",
                AutoSize = false,
                Height = 60,
                Anchor = AnchorStyles.Left | AnchorStyles.Right | AnchorStyles.Top | AnchorStyles.Bottom,
                BorderStyle = BorderStyle.FixedSingle,
                BackColor = Color.LightYellow
            };
            
            // Добавляем элементы в таблицу
            mainTable.Controls.Add(lblX, 0, 0);
            mainTable.Controls.Add(txtX, 1, 0);
            mainTable.Controls.Add(lblY, 0, 1);
            mainTable.Controls.Add(txtY, 1, 1);
            mainTable.Controls.Add(lblZ, 0, 2);
            mainTable.Controls.Add(txtZ, 1, 2);
            mainTable.SetColumnSpan(btnCalculate, 2);
            mainTable.Controls.Add(btnCalculate, 0, 3);
            mainTable.SetColumnSpan(lblResult, 2);
            mainTable.Controls.Add(lblResult, 0, 4);
            
            // Добавляем таблицу на форму
            Controls.Add(mainTable);
        }

        private void CalculateButton_Click(object sender, EventArgs e)
        {
            if (double.TryParse(txtX.Text, out double x) && 
                double.TryParse(txtY.Text, out double y) && 
                double.TryParse(txtZ.Text, out double z))
            {
                // Проверяем, что числа попарно различные
                if (Math.Abs(x - y) < 0.000001  Math.Abs(x - z) < 0.000001  Math.Abs(y - z) < 0.000001)
                {
                    lblResult.Text = "Ошибка: числа должны быть попарно различными";
                    return;
                }

                double originalX = x, originalY = y, originalZ = z;
string operation = "";
                
                if (x + y + z < 1)
                {
                    // Если сумма меньше 1, находим наименьшее из трех чисел
                    double min = Math.Min(x, Math.Min(y, z));
                    
                    if (Math.Abs(min - x) < 0.000001)
                    {
                        x = (y + z) / 2;
                        operation = $"Наименьшее число X = {originalX:F2} заменено полусуммой Y и Z: ({originalY:F2} + {originalZ:F2}) / 2";
                    }
                    else if (Math.Abs(min - y) < 0.000001)
                    {
                        y = (x + z) / 2;
                        operation = $"Наименьшее число Y = {originalY:F2} заменено полусуммой X и Z: ({originalX:F2} + {originalZ:F2}) / 2";
                    }
                    else
                    {
                        z = (x + y) / 2;
                        operation = $"Наименьшее число Z = {originalZ:F2} заменено полусуммой X и Y: ({originalX:F2} + {originalY:F2}) / 2";
                    }
                    
                    lblResult.Text = $"Сумма чисел ({originalX:F2} + {originalY:F2} + {originalZ:F2} = {originalX + originalY + originalZ:F2}) < 1\n{operation}\n\nРезультат:\nX = {x:F2}\nY = {y:F2}\nZ = {z:F2}";
                }
                else
                {
                    // Если сумма >= 1, находим меньшее из X и Y
                    if (x < y)
                    {
                        double oldX = x;
                        x = (y + z) / 2;
                        operation = $"Меньшее из X и Y (X = {oldX:F2}) заменено полусуммой Y и Z: ({originalY:F2} + {originalZ:F2}) / 2";
                    }
                    else
                    {
                        double oldY = y;
                        y = (x + z) / 2;
                        operation = $"Меньшее из X и Y (Y = {oldY:F2}) заменено полусуммой X и Z: ({originalX:F2} + {originalZ:F2}) / 2";
                    }
                    
                    lblResult.Text = $"Сумма чисел ({originalX:F2} + {originalY:F2} + {originalZ:F2} = {originalX + originalY + originalZ:F2}) >= 1\n{operation}\n\nРезультат:\nX = {x:F2}\nY = {y:F2}\nZ = {z:F2}";
                }
            }
            else
            {
                lblResult.Text = "Ошибка: введите корректные действительные числа";
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
            Application.Run(new TransformationForm());
        }
    }
}