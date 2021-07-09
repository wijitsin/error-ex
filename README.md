  public partial class main : Form
    {
        OleDbCommand cmd = new OleDbCommand();
        OleDbConnection cn = new OleDbConnection();
        OleDbDataReader dr;
        private DataSet DS = new DataSet();

        public main()
        {
      

            InitializeComponent();
        }

        private void pictureBox1_Click(object sender, EventArgs e)
        {

            OleDbConnection conn = new OleDbConnection(Properties.Settings.Default.accouConnectionString);
            try
            {

                pnbname.Visible = true;
                pnmenu.Visible = true;
                conn.Open();
                String sql = "Select * From userlog";
                OleDbDataAdapter DA = new OleDbDataAdapter(sql, conn);
                DA.Fill(DS, "userall");

                int count = 0;
                foreach (DataRow DR in DS.Tables["userall"].Rows)
                {
                    count++;
                }

                if (count >= 1)//กรณีมีข้อมูลผลการนับcountจะเป็น1 หรือถ้ามีusername passwordที่เหมือนกันมากกว่า1รายการ ผลการนับก็จะมีค่ามากกว่า1
                {
                    MessageBox.Show("พบข้อมูล");//หรีอใส่ code เปิดหน้าformใหม่
                    combo1.DataSource = DS;
                    combo1.DisplayMember = "userall.suname";
                    combo1.ValueMember = "userall.id";



                }
                else
                {

                    MessageBox.Show("กรุณาตรวจสอบใหม่อีกครั้ง");
                }

            }
            catch (Exception ex)
            {
                MessageBox.Show("error" + ex);
            }
            finally
            {
                conn.Close();
            }
        }

        private void combo1_SelectedIndexChanged(object sender, EventArgs e)
        {
            
            OleDbConnection conn = new OleDbConnection(Properties.Settings.Default.accouConnectionString);
            conn.Open();
            String sql2 = "Select * From userlog where suname='" + combo1.Text + "'";
            OleDbCommand cmd = new OleDbCommand(sql2, conn);
            OleDbDataReader reader = cmd.ExecuteReader();


            if (reader.Read())
            {
                MessageBox.Show(""+ sql2);
                txtname.Text = reader["username"].ToString();
                txtpass.Text = reader["password"].ToString();
                txtsuname.Text = reader["suname"].ToString();
                txtID.Text = reader["ID"].ToString();
            }
        }

        private void button3_Click(object sender, EventArgs e)
        {

        }

        private void button1_Click(object sender, EventArgs e)
        {

           cn.Open();
            OleDbCommand editcmd = new OleDbCommand();
            editcmd.Connection = cn;
            string query = "UPDATE userlog SET username='"+ txtname.Text +"' ,password='" + txtpass.Text + "' ,suname='" + txtsuname.Text + "' where ID=" + txtID.Text + "  ";
            MessageBox.Show(query);
            editcmd.CommandText = query;

            editcmd.ExecuteNonQuery();

            cn.Close();
            


        }

        private void main_Load(object sender, EventArgs e)
        {
            cn.ConnectionString = @"Provider=Microsoft.ACE.OLEDB.12.0;Data Source=D:\wijitsin\VB\app\App\App\accou.accdb";
            cmd.Connection = cn;
        }
    }
}
