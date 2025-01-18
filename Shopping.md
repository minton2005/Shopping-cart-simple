namespace Shopping_cart
{
    public partial class Form1 : Form
    {
        public Form1()
        {
            InitializeComponent();
        }

        private void button1_Click(object sender, EventArgs e)
        {
            try
            {
                double dCash = double.Parse(tb_cash.Text);

                double dBeverageTotal = 0;


                if (chb_Coffee.Checked)
                {
                    dBeverageTotal += GetItemTotal(tb_Coffee_Price.Text, tb_Coffee_Quantity.Text);
                }
                if (chb_Greentea.Checked)
                {
                    dBeverageTotal += GetItemTotal(tb_Greentea_Price.Text, tb_Greentea_Quantity.Text);
                }
                double dGrandTotal = dBeverageTotal;

                if (dCash < dGrandTotal)
                {
                    MessageBox.Show("ยอดเงินไม่เพียงพอต่อการชำระเงิน", "แจ้งเตือน", MessageBoxButtons.OK, MessageBoxIcon.Warning);
                    return;
                }
                double dChange = dCash - dGrandTotal;

                tb_total.Text = dGrandTotal.ToString("F2");
                tb_cash.Text = dChange.ToString("F2");

                CalculateChangeDenominations(dChange);
            }
            catch (FormatException)
            {
                MessageBox.Show("เกิดข้อผิดพลาดในการคำนวณ", "ข้อผิดพลาด", MessageBoxButtons.OK, MessageBoxIcon.Error);
            }

        }
        private double GetItemTotal(string priceText, string quantityText)
        {
            double price = 0, quantity = 0;
            try
            {
                price = double.Parse(priceText);
                quantity = double.Parse(quantityText);
            }
            catch (Exception)
            {
                price = 0;
                quantity = 0;
            }
            return price * quantity;
        }
        private void CalculateChangeDenominations(double change)
        {
            double[] denominations = { 1000, 500, 100, 50, 20, 10, 5, 1, 0.50, 0.25 };
            int[] changeCount = new int[denominations.Length];
            double remainChange = change;

            for (int i = 0; i < denominations.Length; i++)
            {
                changeCount[i] = (int)(remainChange / denominations[i]);
                remainChange %= denominations[i];
            }

            tb_1000.Text = changeCount[0].ToString();
            tb_500.Text = changeCount[1].ToString();
            tb_100.Text = changeCount[2].ToString();
            tb_50.Text = changeCount[3].ToString();
            tb_20.Text = changeCount[4].ToString();
            tb_10.Text = changeCount[5].ToString();
            tb_5.Text = changeCount[6].ToString();
            tb_1.Text = changeCount[7].ToString();
        }
    }
    }

