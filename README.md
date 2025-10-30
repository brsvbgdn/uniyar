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
        }
        
        // ОБНОВЛЯЕМ значения в текстовых полях
        txtX.Text = x.ToString("F2");
        txtY.Text = y.ToString("F2");
        txtZ.Text = z.ToString("F2");
        
        // Выводим информацию о преобразовании
        lblResult.Text = $"Сумма чисел ({originalX:F2} + {originalY:F2} + {originalZ:F2} = {originalX + originalY + originalZ:F2})\n{operation}\n\nРезультат:\nX = {x:F2}\nY = {y:F2}\nZ = {z:F2}";
    }
    else
    {
        lblResult.Text = "Ошибка: введите корректные действительные числа";
    }
}