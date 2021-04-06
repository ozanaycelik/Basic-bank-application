# Basic-bank-application
Bank application with sql
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Data.SqlClient;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Forms;

namespace Banka_Otomasyonu
{
    public partial class Cus_form : Form
    {

        Connection baglan = new Connection();

        public Cus_form()
        {
            InitializeComponent();

        }

        private void kullanıcıToolStripMenuItem_Click(object sender, EventArgs e)
        {

        }

        public void PanelChange(Panel open, Panel close)
        {
            open.Visible = true;
            close.Visible = false;
        }

        private void yeniKayıtToolStripMenuItem_Click(object sender, EventArgs e)
        {

            PanelChange(pnl_usersave, pnl_usermenu);

            PanelChange(pnl_usersave, pnl_userchange);

            PanelChange(pnl_usersave, pnl_customer);
            PanelChange(pnl_usersave, pnl_rapor);

        }

        private void btn_usersave_Click(object sender, EventArgs e)
        {


            if (txt_username.Text == null && txt_password.Text == null && txt_email.Text == null && txt_name.Text == null)
            {
                MessageBox.Show("Boş olan değerler mevcut kontrol ediniz.", "Uyarı", MessageBoxButtons.OK, MessageBoxIcon.Exclamation);
            }

            else
            {
                SqlCommand user_ekle = new SqlCommand(@"insert into Giris_Bilgisi (Tür,Ad,Sifre,İsim,email) values (1,@p1,@p2,@p3,@p4)", baglan.sql());

                user_ekle.Parameters.AddWithValue("@p1", txt_username.Text);
                user_ekle.Parameters.AddWithValue("@p2", txt_password.Text);
                user_ekle.Parameters.AddWithValue("@p3", txt_name.Text);
                user_ekle.Parameters.AddWithValue("@p4", txt_email.Text);

                user_ekle.ExecuteNonQuery();

                MessageBox.Show("Kullanıcı eklenmiştir.", "Bilgi", MessageBoxButtons.OK, MessageBoxIcon.Information);
            }

        }


        private void kullanıcıToolStripMenuItem1_Click(object sender, EventArgs e)
        {

            SqlCommand user_search = new SqlCommand(@"SELECT top 1 * FROM Giris_Bilgisi where Tür=1 and (ad LIKE '%'+@p1+'%' or İsim LIKE '%'+@p1+'%' )", baglan.sql());

            user_search.Parameters.AddWithValue("@p1", txt_search.Text);

            user_search.ExecuteNonQuery();
            SqlDataReader read;
            read = user_search.ExecuteReader();
            // SqlDataReader dr = user_search.ExecuteReader();

            while (read.Read())
            {



                txt_changename.Text = read["İsim"].ToString();
                txt_changemail.Text = read["email"].ToString();
                txt_changeusername.Text = read["ad"].ToString();
                txt_changepassword.Text = read["Sifre"].ToString();

                //PanelChange(pnl_userchange, pnl_usersave);


            }

            PanelChange(pnl_userchange, pnl_usermenu);
            PanelChange(pnl_userchange, pnl_usersave);
            PanelChange(pnl_userchange, pnl_customer);
            PanelChange(pnl_userchange, pnl_rapor);
            PanelChange(pnl_userchange, pnl_islemtürü);
        }

        private void User_Form_Load(object sender, EventArgs e)
        {
            PanelChange(pnl_usermenu, pnl_userchange);
            PanelChange(pnl_usermenu, pnl_rapor);
            PanelChange(pnl_usermenu, pnl_islemtürü);
        }

        private void btn_deleteuser_Click(object sender, EventArgs e)
        {

            SqlCommand user_delete = new SqlCommand(@"DELETE FROM Giris_Bilgisi where Tür=1 and ad=@p1  and  Sifre=@p2 and İsim=@p3 and  email=@p4 ", baglan.sql());

            user_delete.Parameters.AddWithValue("@p1", txt_changeusername.Text);
            user_delete.Parameters.AddWithValue("@p2", txt_changepassword.Text);
            user_delete.Parameters.AddWithValue("@p3", txt_changename.Text);
            user_delete.Parameters.AddWithValue("@p4", txt_changemail.Text);


            user_delete.ExecuteNonQuery();

            MessageBox.Show("Kullanıcı Silindi !", "Bilgi", MessageBoxButtons.OK, MessageBoxIcon.Information);

            PanelChange(pnl_usermenu, pnl_userchange);

        }

        private void button2_Click(object sender, EventArgs e)
        {

            SqlCommand user_update = new SqlCommand(@"Update Giris_Bilgisi set Sifre=@p2 ,İsim=@p3 , email=@p4  where Tür=1 and ad=@p1  ", baglan.sql());

            user_update.Parameters.AddWithValue("@p1", txt_changeusername.Text);
            user_update.Parameters.AddWithValue("@p2", txt_changepassword.Text);
            user_update.Parameters.AddWithValue("@p3", txt_changename.Text);
            user_update.Parameters.AddWithValue("@p4", txt_changemail.Text);


            user_update.ExecuteNonQuery();

            MessageBox.Show("Kullanıcı Güncellendi !", "Bilgi", MessageBoxButtons.OK, MessageBoxIcon.Information);

            PanelChange(pnl_usermenu, pnl_userchange);
        }

        private void yeniHesaToolStripMenuItem_Click(object sender, EventArgs e)
        {
            PanelChange(pnl_customer, pnl_usermenu);
            PanelChange(pnl_customer, pnl_usersave);
            PanelChange(pnl_customer, pnl_userchange);
            PanelChange(pnl_customer, pnl_rapor);
            PanelChange(pnl_customer, pnl_islemtürü);
        }

        private void btn_musteriekle_Click(object sender, EventArgs e)
        {

            if (txt_cusad.Text == "" || txt_cusoyad.Text == "" || txt_musterino.Text == "")
            {
                MessageBox.Show("Boş bilgiler mevcut kontrol ediniz !", "Bilgilendirme", MessageBoxButtons.OK, MessageBoxIcon.Information);
            }
            else
            {
                SqlCommand musteri_ekle = new SqlCommand(@"insert into Giris_Bilgisi (Tür,Ad,İsim,email,Soyad,Adres,cepno) values (2,@p1,@p2,@p3,@p4,@p5,@p6)", baglan.sql());

                musteri_ekle.Parameters.AddWithValue("@p1", txt_musterino.Text);
                musteri_ekle.Parameters.AddWithValue("@p2", txt_cusad.Text);
                musteri_ekle.Parameters.AddWithValue("@p3", mtxmaıl.Text);
                musteri_ekle.Parameters.AddWithValue("@p4", txt_cusoyad.Text);
                musteri_ekle.Parameters.AddWithValue("@p5", rtbx_adres.Text);
                musteri_ekle.Parameters.AddWithValue("@p6", mtxcepno.Text);

                musteri_ekle.ExecuteNonQuery();

                MessageBox.Show("Müşteri başarıyla eklenmiştir !", "Bilgilendirme", MessageBoxButtons.OK, MessageBoxIcon.Information);
            }
        }

        private void btn_güncellemusteri_Click(object sender, EventArgs e)
        {
            SqlCommand musterino_ara = new SqlCommand(@"Select * from Giris_Bilgisi where Tür=2 and ad=@p1", baglan.sql());

            musterino_ara.Parameters.AddWithValue("@p1", txt_musterino.Text);

            SqlDataReader dr = musterino_ara.ExecuteReader();

            if (dr.Read())
            {
                SqlCommand musteri_guncelle = new SqlCommand(@"Update Giris_Bilgisi set İsim=@p2 , email=@p3, Soyad=@p4, cepno=@p5,Adres=@p6  where Tür=2 and ad=@p1  ", baglan.sql());


                musteri_guncelle.Parameters.AddWithValue("@p1", txt_musterino.Text);
                musteri_guncelle.Parameters.AddWithValue("@p2", txt_cusad.Text);
                musteri_guncelle.Parameters.AddWithValue("@p3", mtxmaıl.Text);
                musteri_guncelle.Parameters.AddWithValue("@p4", txt_cusoyad.Text);
                musteri_guncelle.Parameters.AddWithValue("@p5", mtxcepno.Text);
                musteri_guncelle.Parameters.AddWithValue("@p6", rtbx_adres.Text);

                musteri_guncelle.ExecuteNonQuery();

                MessageBox.Show("Müşteri başarıyla güncellenmiştir !", "Bilgilendirme", MessageBoxButtons.OK, MessageBoxIcon.Information);
            }

            else
            {
                MessageBox.Show("İlgili müşteri numarası bulunamadı !", "Bilgilendirme", MessageBoxButtons.OK, MessageBoxIcon.Information);
            }


        }

        private void btn_hesapliste_Click(object sender, EventArgs e)
        {

            combo_hesaplar.Items.Clear();


            SqlCommand hesap_listele = new SqlCommand(@"select * FROM Hesaplar where Musteri_ID = (select ID from Giris_Bilgisi where Ad=@p1 and Tür=2)", baglan.sql());

            hesap_listele.Parameters.AddWithValue("@p1", txt_musterino.Text);

            SqlDataReader dr;

            dr = hesap_listele.ExecuteReader();


            while (dr.Read())
            {
                combo_hesaplar.Items.Add(dr["Hesap_No"]);
            }
            MessageBox.Show("İlgili müşteri numarasına ait hesaplar getirilmiştir !", "Bilgilendirme", MessageBoxButtons.OK, MessageBoxIcon.Information);

            //else
            //{
            //    MessageBox.Show("İlgili müşteri numarasına ait hesap bulunamadı !", "Uyarı", MessageBoxButtons.OK, MessageBoxIcon.Exclamation);
            //}






        }

        private void hesap_sil_Click(object sender, EventArgs e)
        {



            SqlCommand hesabul = new SqlCommand(@"select * from Hesaplar where (Bakiye=0 or Bakiye is null) and Hesap_No=@p1", baglan.sql());

            //var xx = combo_hesaplar.SelectedItem.ToString();

            hesabul.Parameters.AddWithValue("@p1", combo_hesaplar.SelectedItem.ToString());


            SqlDataReader dr  = hesabul.ExecuteReader();

            if (dr.Read())
            {
                SqlCommand hesap_sil = new SqlCommand(@"delete from Hesaplar where (Bakiye=0 or Bakiye is null) and Hesap_No=@p1", baglan.sql());

                hesap_sil.Parameters.AddWithValue("@p1", combo_hesaplar.SelectedItem.ToString());


                hesap_sil.ExecuteNonQuery();

                MessageBox.Show(string.Concat(combo_hesaplar.SelectedItem.ToString(), " nolu hesap silinmiştir !"), "Bilgilendirme", MessageBoxButtons.OK, MessageBoxIcon.Information);

                combo_hesaplar.Items.Clear();
                combo_hesaplar.SelectedItem = "";



                SqlCommand hesap_listele = new SqlCommand(@"select * FROM Hesaplar where Musteri_ID = (select ID from Giris_Bilgisi where Ad=@p1 and Tür=2)", baglan.sql());

                hesap_listele.Parameters.AddWithValue("@p1", txt_musterino.Text);

                SqlDataReader dr2;

                dr2 = hesap_listele.ExecuteReader();


                while (dr2.Read())
                {
                    combo_hesaplar.Items.Add(dr2["Hesap_No"]);
                }
            }
            else
            {
                MessageBox.Show("Hesap no yanlış veya bakiye sıfır değil !", "Uyarı", MessageBoxButtons.OK, MessageBoxIcon.Exclamation);
            }


        }

        private void btn_hesapac_Click(object sender, EventArgs e)
        {
            Random rnd = new Random();
            int hesap_no = rnd.Next(2021000002, 2021000099);

            SqlCommand musterino_ara = new SqlCommand(@"Select * from Giris_Bilgisi where Tür=2 and ad=@p1", baglan.sql());

            musterino_ara.Parameters.AddWithValue("@p1", txt_musterino.Text);

            SqlDataReader dr;
            dr = musterino_ara.ExecuteReader();

            if (dr.Read())
            {
                SqlCommand hesap_olustur = new SqlCommand(@"INSERT INTO Hesaplar (Hesap_No,Musteri_ID) values (@p1,(select ID from Giris_Bilgisi where Ad=@p2))", baglan.sql());


                hesap_olustur.Parameters.AddWithValue("@p1", hesap_no);
                hesap_olustur.Parameters.AddWithValue("@p2", txt_musterino.Text);


                hesap_olustur.ExecuteNonQuery();

                MessageBox.Show(string.Format("{0} hesap no oluşmuştur!", hesap_no), "Bilgilendirme", MessageBoxButtons.OK, MessageBoxIcon.Information);

                combo_hesaplar.Items.Clear();

                combo_hesaplar.SelectedItem = "";

                SqlCommand hesap_listele = new SqlCommand(@"select * FROM Hesaplar where Musteri_ID = (select ID from Giris_Bilgisi where Ad=@p1 and Tür=2)", baglan.sql());

                hesap_listele.Parameters.AddWithValue("@p1", txt_musterino.Text);

                SqlDataReader dr3;

                dr3 = hesap_listele.ExecuteReader();


                while (dr3.Read())
                {
                    combo_hesaplar.Items.Add(dr3["Hesap_No"]);
                }
            }

            
            else
            {
                MessageBox.Show("İlgili müşteri numarası bulunamadı !", "Bilgilendirme", MessageBoxButtons.OK, MessageBoxIcon.Information);
            }

        }

        private void müşteriToolStripMenuItem1_Click(object sender, EventArgs e)
        {

            SqlCommand cus_search = new SqlCommand(@"SELECT top 1 * FROM Giris_Bilgisi where Tür=2 and (ad LIKE '%'+@p1+'%' or Soyad LIKE '%'+@p1+'%' or Adres LIKE '%'+@p1+'%' or email LIKE '%'+@p1+'%' or cepno LIKE '%'+@p1+'%')", baglan.sql());

            cus_search.Parameters.AddWithValue("@p1", txt_search.Text);

            cus_search.ExecuteNonQuery();
            SqlDataReader read;
            read = cus_search.ExecuteReader();
            // SqlDataReader dr = user_search.ExecuteReader();

            while (read.Read())
            {



                txt_cusad.Text = read["İsim"].ToString();
                mtxmaıl.Text = read["email"].ToString();
                txt_musterino.Text = read["ad"].ToString();
                rtbx_adres.Text = read["Adres"].ToString();
                txt_cusoyad.Text = read["Soyad"].ToString();
                mtxcepno.Text = read["cepno"].ToString();
                //PanelChange(pnl_userchange, pnl_usersave);


            }

            PanelChange(pnl_customer, pnl_usermenu);
            PanelChange(pnl_customer, pnl_usersave);
            PanelChange(pnl_customer, pnl_userchange);
            PanelChange(pnl_customer, pnl_rapor);
            PanelChange(pnl_customer, pnl_islemtürü);
        }



        private void bankaRaporToolStripMenuItem_Click(object sender, EventArgs e)
        {
            PanelChange(pnl_usermenu, pnl_islemtürü);
            PanelChange(pnl_rapor, pnl_usersave);
            PanelChange(pnl_rapor, pnl_userchange);
            PanelChange(pnl_rapor, pnl_customer);

            SqlCommand komut = new SqlCommand("Select * From Hareket ", baglan.sql());
            SqlDataAdapter da = new SqlDataAdapter(komut);
            DataTable dt = new DataTable();
            da.Fill(dt);
            dataGridView1.DataSource = dt;
        }

        private void btn_gelirgider_Click(object sender, EventArgs e)
        {

            SqlCommand komut = new SqlCommand("Select * From Hareket where YEAR(Tarih)=YEAR(@p1) AND MONTH(Tarih)=MONTH(@p1) AND  DAY(Tarih)=DAY(@p1) ", baglan.sql());
            komut.Parameters.AddWithValue("@p1", dateTimePicker1.Value.Date);
            SqlDataAdapter da = new SqlDataAdapter(komut);
            DataTable dt = new DataTable();
            da.Fill(dt);
            dataGridView1.DataSource = dt;
        }

        private void btn_islemSil_Click(object sender, EventArgs e)
        {
           

            cbx_islemtürüSil.Items.RemoveAt(cbx_islemtürüSil.SelectedIndex);

           
        }

        private void btn_islemEkle_Click(object sender, EventArgs e)
        {
            cbx_islemtürüSil.Items.Add(txt_islemEkle.Text);

            txt_islemEkle.Text = "";
        }

        private void işlemTürüToolStripMenuItem_Click(object sender, EventArgs e)
        {
            PanelChange(pnl_usermenu, pnl_usersave);
            PanelChange(pnl_islemtürü, pnl_usersave);
            PanelChange(pnl_islemtürü, pnl_customer);
            PanelChange(pnl_islemtürü, pnl_rapor);
            PanelChange(pnl_islemtürü, pnl_userchange);
        }

        private void pnl_islemtürü_Paint(object sender, PaintEventArgs e)
        {

        }

        private void label2_Click(object sender, EventArgs e)
        {

        }
    }
}
