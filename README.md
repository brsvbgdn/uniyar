partial class MainForm
{
    private System.ComponentModel.IContainer components = null;

    protected override void Dispose(bool disposing)
    {
        if (disposing && (components != null))
        {
            components.Dispose();
        }
        base.Dispose(disposing);
    }

    private void InitializeComponent()
    {
        this.txtInput = new System.Windows.Forms.TextBox();
        this.btnCalculate = new System.Windows.Forms.Button();
        this.lblResult = new System.Windows.Forms.Label();
        this.lblPrompt = new System.Windows.Forms.Label();
        this.SuspendLayout();
        
        // txtInput
        this.txtInput.Location = new System.Drawing.Point(15, 35);
        this.txtInput.Name = "txtInput";
        this.txtInput.Size = new System.Drawing.Size(200, 20);
        this.txtInput.TabIndex = 0;
        
        // btnCalculate
        this.btnCalculate.Location = new System.Drawing.Point(15, 70);
        this.btnCalculate.Name = "btnCalculate";
        this.btnCalculate.Size = new System.Drawing.Size(200, 30);
        this.btnCalculate.TabIndex = 1;
        this.btnCalculate.Text = "Определить число сотен";
        this.btnCalculate.UseVisualStyleBackColor = true;
        this.btnCalculate.Click += new System.EventHandler(this.btnCalculate_Click);
        
        // lblResult
        this.lblResult.AutoSize = true;
        this.lblResult.Font = new System.Drawing.Font("Microsoft Sans Serif", 10F);
        this.lblResult.Location = new System.Drawing.Point(12, 110);
        this.lblResult.Name = "lblResult";
        this.lblResult.Size = new System.Drawing.Size(0, 17);
        this.lblResult.TabIndex = 2;
        
        // lblPrompt
        this.lblPrompt.AutoSize = true;
        this.lblPrompt.Location = new System.Drawing.Point(12, 15);
        this.lblPrompt.Name = "lblPrompt";
        this.lblPrompt.Size = new System.Drawing.Size(203, 13);
        this.lblPrompt.TabIndex = 3;
        this.lblPrompt.Text = "Введите натуральное число (>99):";
        
        // MainForm
        this.AutoScaleDimensions = new System.Drawing.SizeF(6F, 13F);
        this.AutoScaleMode = System.Windows.Forms.AutoScaleMode.Font;
        this.ClientSize = new System.Drawing.Size(234, 161);
        this.Controls.Add(this.lblPrompt);
        this.Controls.Add(this.lblResult);
        this.Controls.Add(this.btnCalculate);
        this.Controls.Add(this.txtInput);
        this.Name = "MainForm";
        this.Text = "Счетчик сотен";
        this.ResumeLayout(false);
        this.PerformLayout();
    }

    private TextBox txtInput;
    private Button btnCalculate;
    private Label lblResult;
    private Label lblPrompt;
}