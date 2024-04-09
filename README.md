# c-4

using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Forms;
using System.Data.SqlClient;

namespace TestBenzinIstasyonu
{
    public partial class Form1 : Form
    {
        public Form1()
        {
            InitializeComponent();
        }
        SqlConnection baglanti =new SqlConnection(@"Data Source=NTB1000088;Initial Catalog=DbBenzinIstasyon;User ID=sa;Password=sa321sa?;");
        
        void listele()
        {
            baglanti.Open();
            SqlCommand komut = new SqlCommand("Select * From TblBenzin  where PetrolTur='Kurşunsuz95'", baglanti);
            SqlDataReader dr = komut.ExecuteReader();
            while (dr.Read())
            {
                lblKursunsuz95.Text = dr[3].ToString();
                progressBar1.Value = int.Parse(dr[4].ToString());
                lblKursunsuz95Litre.Text = dr[4].ToString();
            }
            baglanti.Close();

            baglanti.Open();
            SqlCommand komut2 = new SqlCommand("Select * From TblBenzin where PetrolTur='Kurşunsuz97'", baglanti);
            SqlDataReader dr2 = komut2.ExecuteReader();
            while (dr2.Read())
            {
                lblKursunsuz97.Text = dr2[3].ToString();
                progressBar2.Value = int.Parse(dr2[4].ToString());
                lblKursunsuz97Litre.Text = dr2[4].ToString();
            }
            baglanti.Close();

            baglanti.Open();
            SqlCommand komut3 = new SqlCommand("Select * From TblBenzin where PetrolTur='EuroDizel10'", baglanti);
            SqlDataReader dr3 = komut3.ExecuteReader();
            while (dr3.Read())
            {
                lblEuroDizel10.Text = dr3[3].ToString();
                progressBar3.Value = int.Parse(dr3[4].ToString());
                lblEuroDizelLitre.Text = dr3[4].ToString();
            }
            baglanti.Close();

            baglanti.Open();
            SqlCommand komut4 = new SqlCommand("Select * From TblBenzin where PetrolTur='YeniProDizel'", baglanti);
            SqlDataReader dr4 = komut4.ExecuteReader();
            while (dr4.Read())
            {
                lblYeniProDizel.Text = dr4[3].ToString();
                progressBar4.Value = int.Parse(dr4[4].ToString());
                lblYeniProDizelLitre.Text = dr4[4].ToString();
            }
            baglanti.Close();

            baglanti.Open();
            SqlCommand komut5 = new SqlCommand("Select * From TblBenzin where PetrolTur='Gaz'", baglanti);
            SqlDataReader dr5 = komut5.ExecuteReader();
            while (dr5.Read())
            {
                lblGaz.Text = dr5[3].ToString();
                progressBar5.Value = int.Parse(dr5[4].ToString());
                lblGazLitre.Text = dr5[4].ToString();
            }
            baglanti.Close();
        }

        private void Form1_Load(object sender, EventArgs e)
        {
            listele();
        }

        private void numericUpDown1_ValueChanged(object sender, EventArgs e)
        {
            double kursunsuz95, litre, tutar;
            kursunsuz95= Convert.ToDouble(lblKursunsuz95.Text);
            litre = Convert.ToDouble(numericUpDown1.Value);
            tutar=kursunsuz95*litre;
            txtKursunsuz95Fiyat.Text=tutar.ToString();
        }

        private void numericUpDown2_ValueChanged(object sender, EventArgs e)
        {
            double kursunsuz97, litre, tutar;   
            kursunsuz97= Convert.ToDouble(lblKursunsuz97.Text);
            litre=Convert.ToDouble(numericUpDown2.Value);
            tutar = kursunsuz97 * litre;
            txtKursunsuz97Fiyat.Text=tutar.ToString();
        }

        private void numericUpDown3_ValueChanged(object sender, EventArgs e)
        {
            double eurodizel, litre, tutar;
            eurodizel=Convert.ToDouble(lblEuroDizel10.Text);
            litre=Convert.ToDouble(numericUpDown3.Value);
            tutar=eurodizel*litre;
            txtKursunsuzEuroDizelFiyat.Text=tutar.ToString();
        }

        private void numericUpDown4_ValueChanged(object sender, EventArgs e)
        {
            double yeniprodizel, litre, tutar;
            yeniprodizel = Convert.ToDouble(lblYeniProDizel.Text);
            litre= Convert.ToDouble(numericUpDown4.Value);
            tutar = yeniprodizel * litre;
            txtYeniProDizelFiyat.Text=tutar.ToString();
        }

        private void numericUpDown5_ValueChanged(object sender, EventArgs e)
        {
            double gaz, litre, tutar;
            gaz=Convert.ToDouble(lblGaz.Text);
            litre=Convert.ToDouble(numericUpDown5.Value);
            tutar = gaz * litre;
            txtGazFiyat.Text=tutar.ToString();
        }

        private void btnDepoDoldur_Click(object sender, EventArgs e)
        {
            if (numericUpDown1.Value != 0)
            {
                baglanti.Open();
                SqlCommand komut = new SqlCommand("insert into TblHareket (plaka,benzintur,litre,fiyat) values (@p1,@p2,@p3,@p4)", baglanti);
                komut.Parameters.AddWithValue("@p1",txtPlaka.Text);
                komut.Parameters.AddWithValue("@p2", "Kurşunsuz 95");
                komut.Parameters.AddWithValue("@p3", numericUpDown1.Value);
                komut.Parameters.AddWithValue("@p4",decimal.Parse(txtKursunsuz95Fiyat.Text));
                komut.ExecuteNonQuery();
                baglanti.Close();
                

                baglanti.Open() ;
                SqlCommand komut2 = new SqlCommand("update TblKasa set miktar=miktar+@p1", baglanti);
                komut2.Parameters.AddWithValue("@p1",decimal.Parse(txtKursunsuz95Fiyat.Text));
                komut2.ExecuteNonQuery();
                baglanti.Close();
                


                baglanti.Open();
                SqlCommand komut3 = new SqlCommand("update TblBenzin set stok=stok-@p1 where PetrolTur='Kurşunsuz95'",baglanti);
                komut3.Parameters.AddWithValue("@p1", numericUpDown1.Value);
                komut3.ExecuteNonQuery();
                baglanti.Close();
                MessageBox.Show("Satış yapıldı");
                listele();
            }
        }
    }
}
