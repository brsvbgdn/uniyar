using System;
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
            this.ClientSize = new System.Drawing.Size(1000, 700);
            this.Text = "Graph of Function y = 10^-3 * |x|^(5/2) + ln|x+35.4|";
            this.StartPosition = FormStartPosition.CenterScreen;
            this.ResumeLayout(false);
        }

        private void InitializeChart()
        {
            chart1 = new Chart();
            chart1.Dock = DockStyle.Fill;
            
            ChartArea chartArea = new ChartArea();
            chartArea.AxisX.Title = "X";
            chartArea.AxisY.Title = "Y";
            chartArea.AxisX.MajorGrid.Enabled = true;
            chartArea.AxisY.MajorGrid.Enabled = true;
            chart1.ChartAreas.Add(chartArea);

            Series series = new Series();
            series.Name = "Function";
            series.ChartType = SeriesChartType.Line;
            series.Color = System.Drawing.Color.Red;
            series.BorderWidth = 2;
            series.MarkerStyle = MarkerStyle.Circle;
            series.MarkerSize = 5;
            series.MarkerColor = System.Drawing.Color.Blue;
            
            chart1.Series.Add(series);

            Legend legend = new Legend();
            legend.Docking = Docking.Top;
            chart1.Legends.Add(legend);

            this.Controls.Add(chart1);
        }

        private void PlotFunction()
        {
            double x0 = 1.75;
            double xk = -2.5;
            double dx = -0.25;
            double b = 35.4;

            chart1.Series["Function"].Points.Clear();

            for (double x = x0; x >= xk; x += dx)
            {
                try
                {
                    double y = 1e-3 * Math.Pow(Math.Abs(x), 2.5) + Math.Log(Math.Abs(x + b));
                    chart1.Series["Function"].Points.AddXY(x, y);
                }
                catch (Exception ex)
                {
                    Console.WriteLine($"Error at x={x}: {ex.Message}");
                }
            }

            chart1.Titles.Clear();
            Title title = new Title("y = 10⁻³ * |x|^(5/2) + ln|x+35.4|", 
                                  Docking.Top, 
                                  new System.Drawing.Font("Arial", 12, System.Drawing.FontStyle.Bold), 
                                  System.Drawing.Color.Black);
            chart1.Titles.Add(title);
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