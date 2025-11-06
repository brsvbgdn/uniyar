using System;
using System.Collections.Generic;
using System.Windows.Forms;
using System.Windows.Forms.DataVisualization.Charting;

namespace FunctionChartApp
{
    public partial class Form1 : Form
    {
        private Chart chart1;
        
        public Form1()
        {
            InitializeComponent();
            InitializeChart();
            PlotFunction();
        }

        private void InitializeComponent()
        {
            this.SuspendLayout();
            
            // Настройка формы
            this.ClientSize = new System.Drawing.Size(1000, 700);
            this.Text = "Graph of Function y = 10^-3 * |x|^(5/2) + ln|x+35.4|";
            this.StartPosition = FormStartPosition.CenterScreen;
            
            this.ResumeLayout(false);
        }

        private void InitializeChart()
        {
            // Создаем и настраиваем Chart
            chart1 = new Chart();
            chart1.Dock = DockStyle.Fill;
            
            // Создаем область для графика
            ChartArea chartArea = new ChartArea();
            chartArea.AxisX.Title = "X";
            chartArea.AxisY.Title = "Y";
            chartArea.AxisX.MajorGrid.Enabled = true;
            chartArea.AxisY.MajorGrid.Enabled = true;
            chartArea.AxisX.MinorGrid.Enabled = false;
            chartArea.AxisY.MinorGrid.Enabled = false;
            
            // Настраиваем внешний вид осей
            chartArea.AxisX.TitleFont = new System.Drawing.Font("Arial", 10, System.Drawing.FontStyle.Bold);
            chartArea.AxisY.TitleFont = new System.Drawing.Font("Arial", 10, System.Drawing.FontStyle.Bold);
            chartArea.AxisX.LabelStyle.Font = new System.Drawing.Font("Arial", 8);
            chartArea.AxisY.LabelStyle.Font = new System.Drawing.Font("Arial", 8);
            
            chart1.ChartAreas.Add(chartArea);

            // Создаем серию для графика
            Series series = new Series();
            series.Name = "Function";
            series.ChartType = SeriesChartType.Line;
            series.Color = System.Drawing.Color.Red;
            series.BorderWidth = 2;
            series.MarkerStyle = MarkerStyle.Circle;
            series.MarkerSize = 5;
            series.MarkerColor = System.Drawing.Color.Blue;
            
            chart1.Series.Add(series);

            // Добавляем легенду
            Legend legend = new Legend();
            legend.Docking = Docking.Top;
            legend.Alignment = System.Drawing.StringAlignment.Center;
            chart1.Legends.Add(legend);

            // Добавляем Chart на форму
            this.Controls.Add(chart1);
        }

        private void PlotFunction()
        {
            double x0 = 1.75;
            double xk = -2.5;
            double dx = -0.25;
            double b = 35.4;

            List<double> xValues = new List<double>();
            List<double> yValues = new List<double>();

            // Вычисляем точки графика
            for (double x = x0; x >= xk; x += dx)
            {
                try
                {
                    // y = 10^-3 * |x|^(5/2) + ln|x+b|
                    double y = 1e-3 * Math.Pow(Math.Abs(x), 2.5) + Math.Log(Math.Abs(x + b));
                    
                    xValues.Add(x);
                    yValues.Add(y);
                    
                    Console.WriteLine($"x = {x:F2}, y = {y:F4}");
                }
                catch (Exception ex)
                {
                    Console.WriteLine($"Error at x={x}: {ex.Message}");
                }
            }

            // Очищаем существующие данные
            chart1.Series["Function"].Points.Clear();

            // Добавляем точки на график
            for (int i = 0; i < xValues.Count; i++)
            {
                chart1.Series["Function"].Points.AddXY(xValues[i], yValues[i]);
            }

            // Настраиваем заголовок
            chart1.Titles.Clear();
            Title title = new Title("y = 10⁻³ * |x|^(5/2) + ln|x+35.4|", 
                                  Docking.Top, 
                                  new System.Drawing.Font("Arial", 12, System.Drawing.FontStyle.Bold), 
                                  System.Drawing.Color.Black);
            chart1.Titles.Add(title);

            // Обновляем график
            chart1.Invalidate();
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