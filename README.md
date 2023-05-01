# The-13th-Gwangju-Gwangsan-gu-Administration-dong-Weather-Search-project
C# Weather query using winforms and RSS system
![image](https://user-images.githubusercontent.com/115389450/235431352-2cea0e08-a9c0-49db-9480-6a728aa59326.png)
![image](https://user-images.githubusercontent.com/115389450/235431393-0ef3d301-924d-419c-8f6c-df874286604b.png)
```
using MySql.Data.MySqlClient;
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Forms;

namespace WindowsFormsApp2
{
    public partial class Loginform : Form
    {
        public Loginform()
        {
            InitializeComponent();
            this.PasswordText.AutoSize = false;
            this.PasswordText.Size = new Size(this.PasswordText.Width, 64);

        }

        private void ExitLabel_Click(object sender, EventArgs e)
        {
            //this.Close();
            Application.Exit();
        }

        private void ExitLabel_MouseEnter(object sender, EventArgs e)
        {
            ExitLabel.ForeColor = Color.Green;
        }

        private void ExitLabel_MouseLeave(object sender, EventArgs e)
        {
            ExitLabel.ForeColor = Color.White;
        }

        Point lastPoint; // 포인트는 설정을 위한 특수 클래스
        private void panel1_MouseMove(object sender, MouseEventArgs e)
        {
            if(e.Button == MouseButtons.Left) // 여기서 e는 생성된 객체이다 ! 어렵게 생각 ㄴ
            {
                this.Left += e.X - lastPoint.X;
                this.Top += e.Y - lastPoint.Y;
            }
        }

        private void panel1_MouseDown(object sender, MouseEventArgs e)
        {
            lastPoint = new Point(e.X, e.Y); // 마우스를 아래로 이동할 때 새로운 점 좌표를 얻는다.

        }

        private void btnlogin_Click(object sender, EventArgs e)
        {
            String loginUser = IdText.Text;
            String passUser = PasswordText.Text;

            DB db = new DB(); // 메모리 할당

            DataTable table = new DataTable(); // 메모리 할당

            MySqlDataAdapter adapter = new MySqlDataAdapter(); // 메모리 할당

            MySqlCommand command = new MySqlCommand("SELECT * FROM loginuser WHERE `login` = @uL AND `password` = @uP", db.getConnection()); // 메모리 할당 , 이 줄은 sql 명령 자체와 같다.

            command.Parameters.Add("@uL", MySqlDbType.VarChar).Value = loginUser;
            command.Parameters.Add("@uP", MySqlDbType.VarChar).Value = passUser;

            adapter.SelectCommand = command; // 특정 명령을 실행한 다음
            adapter.Fill(table); // 모든 데이터는 내부에서 변형되도록 한다.

            if (table.Rows.Count > 0)
            {
                MessageBox.Show("환영합니다.", "로그인성공", MessageBoxButtons.OK, MessageBoxIcon.Information);
                this.Hide();
                MainForm mainForm = new MainForm();
                mainForm.Show();
            }
                
            else
                MessageBox.Show("계정이 등록되어 있지 않습니다.", "로그인실패",MessageBoxButtons.OK ,MessageBoxIcon.Error);


        }

        private void RegisterLabel_Click(object sender, EventArgs e)
        {
            this.Hide();
            RegisterForm registerForm = new RegisterForm(); // 메모리 할당
            registerForm.Show();
        }
    }
}
```
![image](https://user-images.githubusercontent.com/115389450/235431447-c94c7864-f95b-47d0-b60b-e71ecbee2143.png)
```
using MySql.Data.MySqlClient;
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Forms;

namespace WindowsFormsApp2
{
    public partial class RegisterForm : Form
    {
        public RegisterForm()
        {
            InitializeComponent();

            this.PasswordText.AutoSize = false;
            this.PasswordText.Size = new Size(this.PasswordText.Width, 64);

            userNameField.Text = "이름을 입력하세요.";
            userNameField.ForeColor = Color.Gray;

            userPhoneField.Text = "연락처를 입력하세요.";
            userPhoneField.ForeColor = Color.Gray;

            IdText.Text = "아이디를 입력하세요.";
            IdText.ForeColor = Color.Gray;

        }

        private void ExitLabel_Click(object sender, EventArgs e)
        {
            //this.Close();
            Application.Exit();
        }

        Point lastPoint;
        private void panel1_MouseMove(object sender, MouseEventArgs e)
        {
            if (e.Button == MouseButtons.Left)
            {
                this.Left += e.X - lastPoint.X;
                this.Top += e.Y - lastPoint.Y;
            }
        }

        private void panel1_MouseDown(object sender, MouseEventArgs e)
        {
            lastPoint = new Point(e.X, e.Y);
        }

        private void userNameField_Enter(object sender, EventArgs e)
        {
            if (userNameField.Text == "이름을 입력하세요.")
            {
                userNameField.Text = "";
                userNameField.ForeColor = Color.Black;
            }
        }

        private void userNameField_Leave(object sender, EventArgs e)
        {
            if (userNameField.Text == "")
            {
                userNameField.Text = "이름을 입력하세요.";
                userNameField.ForeColor = Color.Gray;
            }
        }

        private void IdText_Enter(object sender, EventArgs e)
        {
            if (IdText.Text == "아이디를 입력하세요.")
            {
                IdText.Text = "";
                IdText.ForeColor = Color.Black;
            }
        }

        private void IdText_Leave(object sender, EventArgs e)
        {
            if (IdText.Text == "")
            {
                IdText.Text = "아이디를 입력하세요.";
                IdText.ForeColor = Color.Gray;
            }
        }

        private void userPhoneField_Enter(object sender, EventArgs e)
        {
            if (userPhoneField.Text == "연락처를 입력하세요.")
            {
                userPhoneField.Text = "";
                userPhoneField.ForeColor = Color.Black;
            }
        }

        private void userPhoneField_Leave(object sender, EventArgs e)
        {
            if (userPhoneField.Text == "")
            {
                userPhoneField.Text = "연락처를 입력하세요.";
                userPhoneField.ForeColor = Color.Gray;
            }
        }

        private void btnRegister_Click(object sender, EventArgs e)
        {
            if (userNameField.Text == "이름을 입력하세요." || userPhoneField.Text == "연락처를 입력하세요." || IdText.Text == "아이디를 입력하세요." || PasswordText.Text == "")
            {
                MessageBox.Show("모든 항목을 입력 해 주세요", "등록실패", MessageBoxButtons.OK, MessageBoxIcon.Error);
                return;
            }


            if (isUserExists()) // 아이디가 존재할 경우 종료
                return;

            DB db = new DB();
            MySqlCommand command = new MySqlCommand("INSERT INTO `loginuser` (`login`, `password`, `name`, `phone`) VALUES (@login, @pass, @name, @phone)", db.getConnection());

            command.Parameters.Add("@login", MySqlDbType.VarChar).Value = IdText.Text;
            command.Parameters.Add("@pass", MySqlDbType.VarChar).Value = PasswordText.Text;
            command.Parameters.Add("@name", MySqlDbType.VarChar).Value = userNameField.Text;
            command.Parameters.Add("@phone", MySqlDbType.VarChar).Value = userPhoneField.Text;

            db.openConnection();

            if (command.ExecuteNonQuery() == 1)
                MessageBox.Show("계정이 등록되었습니다.", "등록성공", MessageBoxButtons.OK, MessageBoxIcon.Information);
            else
                MessageBox.Show("계정이 등록되지 않았습니다.");

            db.closeConnection();

        }

        public Boolean isUserExists()
        {
            DB db = new DB();

            DataTable table = new DataTable();

            MySqlDataAdapter adapter = new MySqlDataAdapter();

            MySqlCommand command = new MySqlCommand("SELECT * FROM `loginuser` WHERE `login` = @uL", db.getConnection());

            command.Parameters.Add("@uL", MySqlDbType.VarChar).Value = IdText.Text;

            adapter.SelectCommand = command;
            adapter.Fill(table);

            if (table.Rows.Count > 0)
            {
                MessageBox.Show("이미 존재하는 아이디입니다.", "등록실패", MessageBoxButtons.OK, MessageBoxIcon.Error); ;
                return true;
            }

            else
                  
                return false;

        }

        private void LoginLabel_Click(object sender, EventArgs e)
        {
            this.Hide();
            Loginform loginForm = new Loginform();
            loginForm.Show();
        }
    }
}
```
![image](https://user-images.githubusercontent.com/115389450/235431533-ec46666f-a513-4df7-9309-50439314fb68.png)
![image](https://user-images.githubusercontent.com/115389450/235431616-5a091642-79e1-4378-9461-f2a71e80223c.png)
![image](https://user-images.githubusercontent.com/115389450/235431647-ace9fcf9-5c81-48ab-93f8-ad2d12d285d2.png)
![image](https://user-images.githubusercontent.com/115389450/235431684-1515cf68-1981-4aac-a58e-917d3f0d347c.png)
![image](https://user-images.githubusercontent.com/115389450/235431715-80a545f5-10b6-4ee6-a3f9-0377b2cc64ec.png)
```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using MySql.Data.MySqlClient; // MYSQL 연결할 첫번째 단계

namespace WindowsFormsApp2
{
    internal class DB
    {
        MySqlConnection connection =  new MySqlConnection
            ("server=localhost; " +
            "port=3306;" +
            "username=root;" +
            "password=0000;" +
            "database=users"); //

        public void openConnection()
        {
            if (connection.State == System.Data.ConnectionState.Closed)
                connection.Open();
        }

        public void closeConnection()
        {
            if (connection.State == System.Data.ConnectionState.Open)
                connection.Close();
        }

        public MySqlConnection getConnection()
        {
            return connection; // 단순히 연결 자체를 반환한다.
        }
    }
}
```
### 메인함수, 실행담당
```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Threading.Tasks;
using System.Windows.Forms;

namespace WindowsFormsApp2
{
    internal static class Program
    {
        /// <summary>
        /// 해당 애플리케이션의 주 진입점입니다.
        /// </summary>
        [STAThread]
        static void Main()
        {
            Application.EnableVisualStyles();
            Application.SetCompatibleTextRenderingDefault(false);
            Application.Run(new Loginform());
            //Application.Run(new RegisterForm());
        }
    }
}
```
![image](https://user-images.githubusercontent.com/115389450/235431839-8cf9d89f-7062-41d1-aede-9b2fbdfcc94a.png)
```
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.IO;
using System.Linq;
using System.Net;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Forms;
using System.Windows.Forms.DataVisualization.Charting;
using System.Xml;

namespace WindowsFormsApp2
{
    public partial class MainForm : Form
    {

        // 그림 불러오기 경로참조 : bin -> debug -> images
        Bitmap sunny = new Bitmap("images/sunny.png");
        Bitmap cloud = new Bitmap("images/cloud.png");
        Bitmap manycloud = new Bitmap("images/manycloud.png");
        Bitmap rainy = new Bitmap("images/rainy.png");
        Bitmap raninysnow = new Bitmap("images/raninysnow.png");
        Bitmap snow = new Bitmap("images/snow.png");
        Bitmap shower = new Bitmap("images/shower.png");

        public MainForm()
        {
            InitializeComponent();
        }

        private void ExitLabel_Click(object sender, EventArgs e)
        {
            //this.Close();
            Application.Exit();
        }

        private void btnrequest_Click(object sender, EventArgs e)
        {
            String dong = cbarea.SelectedItem.ToString();
            String dong_number = "";

            switch (dong)
            {
                case "도산동":
                    dong_number = "2920054000";
                    break;

                case "동곡동":
                    dong_number = "2920066000";
                    break;

                case "본량동":
                    dong_number = "2920069000";
                    break;

                case "비아동":
                    dong_number = "2920062000";
                    break;

                case "삼도동":
                    dong_number = "2920068000";
                    break;

                case "송정1동":
                    dong_number = "2920051500";
                    break;

                case "송정2동":
                    dong_number = "2920052500";
                    break;

                case "수완동":
                    dong_number = "2920063700";
                    break;

                case "신가동":
                    dong_number = "2920063000";
                    break;

                case "신창동":
                    dong_number = "2920070000";
                    break;

                case "신흥동":
                    dong_number = "2920055000";
                    break;

                case "어룡동":
                    dong_number = "2920056500";
                    break;

                case "우산동":
                    dong_number = "2920058000";
                    break;

                case "운남동":
                    dong_number = "2920063500";
                    break;

                case "월곡1동":
                    dong_number = "2920063500";
                    break;

                case "월곡2동":
                    dong_number = "2920061000";
                    break;

                case "임곡동":
                    dong_number = "2920065000";
                    break;

                case "첨단1동":
                    dong_number = "2920062400";
                    break;

                case "첨단2동":
                    dong_number = "2920062600";
                    break;

                case "평동":
                    dong_number = "2920067000";
                    break;
            }
            String query = "http://www.kma.go.kr/wid/queryDFSRSS.jsp?zone=" + dong_number;

            // 먼저 클라이언트에서 Request 한다.
            WebRequest request = WebRequest.Create(query);

            request.Method = "GET";
            // Respons를 받는다.

            WebResponse response = request.GetResponse();

            Stream stream = response.GetResponseStream();

            StreamReader reader = new StreamReader(stream);

            string answer = reader.ReadToEnd();

            //richTextBox1.Text = answer;


            // Respons 받은 것을 xml으로 파싱한다.

            XmlDocument xmlDo = new XmlDocument();
            xmlDo.LoadXml(answer); // xml 문서 파싱할 준비

            // 제목이랑 중요한 정보 처리 하는 부분
            XmlNode channel = xmlDo["rss"]["channel"];
            arealabel.Text = channel["title"].InnerText;
            datelabel.Text = channel["pubDate"].InnerText;
            //MessageBox.Show(channel["title"].InnerText);


            // 데이터 처리하는 부분
            XmlNode xmlNode = xmlDo["rss"]["channel"]["item"]["description"]["body"]; // xml 문서에서 필요한 데이터를 파고 들어 찾기 위한 스트링 선언
                                                                                      // rss에 달린 그 가지들 중에 묶여있는 가지를 초이스 해야 한다. channel, item, description, body 는 가지이다.

            /* richTextBox1.Text = xmlNode.ChildNodes.Count.ToString();*/ // chileNodes 란 프로퍼티에 주목. 개수가 몇개인지 파악.

            //for(int i=0; i<xmlNode.ChildNodes.Count; i++)
            //{
            //    richTextBox1.Text += xmlNode.ChildNodes[i]["hour"].InnerText + "\n"; // xml 가지 중에 시간 가지 출력
            //}
            Label[] hour = { label2, label9, label16, label23, label30, label37, label44, label51, label58 };
            Label[] temp = { label3, label10, label17, label24, label31, label38, label45, label52, label59 };
            Label[] wfKor = { label4, label11, label18, label25, label32, label39, label46, label53, label60 };
            Label[] pop = { label5, label12, label19, label26, label33, label40, label47, label54, label61 };
            Label[] ws = { label6, label13, label20, label27, label34, label41, label48, label55, label62 };
            Label[] wdKor = { label7, label14, label21, label28, label35, label42, label49, label56, label63 };
            Label[] reh = { label8, label15, label22, label29, label36, label43, label50, label57, label64 };

            PictureBox[] pb = { pictureBox2, pictureBox3, pictureBox4, pictureBox5, pictureBox6, pictureBox7, pictureBox8, pictureBox9, pictureBox10, };

            // 원래 차트에 그려져 있던 내용을 초기화 하기
            chart1.Series[0].Points.Clear();


            // 차트의 디자인 설정하기

            CustomLabel[] cl = new CustomLabel[9];

            for (int i = 0; i < 9; i++) // 21시부터 다음날 21시까지 총 9건이 나옴.
            {
                /*richTextBox1.Text += xmlNode.ChildNodes[i]["hour"].InnerText + "\n";*/ // xml 가지 중에 시간 가지 출력
                hour[i].Text = "시간: " + xmlNode.ChildNodes[i]["hour"].InnerText + "시"; // 시간
                pop[i].Text = "강수확률: " + xmlNode.ChildNodes[i]["pop"].InnerText + "%"; // 강수확률
                ws[i].Text = "풍속: " + xmlNode.ChildNodes[i]["ws"].InnerText.ToString() + "m/s"; // 풍속
                wdKor[i].Text = "풍향: " + xmlNode.ChildNodes[i]["wdKor"].InnerText; // 풍향
                reh[i].Text = "습도: " + xmlNode.ChildNodes[i]["reh"].InnerText + "%"; // 습도

                temp[i].Text = "기온: " + xmlNode.ChildNodes[i]["temp"].InnerText + "'C"; // 기온
                // 기온으로 그래프 그려 보기
                // y값 확보하기
                double graph_temp = double.Parse(xmlNode.ChildNodes[i]["temp"].InnerText); // 더블 파스로 묶어주면 더블로 변환
                // x값 확보하기
                string graph_time = xmlNode.ChildNodes[i]["hour"].InnerText + "시";

                chart1.Series[0].Points.AddXY(i, graph_temp); // 차트 데이터를 관장하는 Series

                cl[i] = new CustomLabel();
                cl[i].Text = graph_time;
                cl[i].FromPosition = i - 1;
                cl[i].ToPosition = i + 1;
                chart1.ChartAreas[0].AxisX.CustomLabels.Add(cl[i]);


                string wf = xmlNode.ChildNodes[i]["wfKor"].InnerText;
                wfKor[i].Text = "날씨: " + wf; // 날씨

                // 날씨에 따라서 그림을 다르게 표시 해 보기.

                if(wf == "맑음")
                {
                    pb[i].Image = sunny;
                }
                else if(wf == "구름 많음")
                {
                    pb[i].Image = cloud;
                }
                else if (wf == "흐림")
                {
                    pb[i].Image = manycloud;
                }
                else if (wf == "비")
                {
                    pb[i].Image = rainy;
                }
                else if (wf == "비/눈")
                {
                    pb[i].Image = raninysnow;
                }
                else if (wf == "눈")
                {
                    pb[i].Image = snow;
                }
                else if (wf == "소나기")
                {
                    pb[i].Image = shower;
                }
            }
            /*
             * 시간 hour
             * 기온 temp
             * 날씨 wfKor - 맑음, 구름많음, 흐림, 비, 비/눈, 눈, 소나기
             * 강수확률 pop
             * 풍속 ws
             * 풍향 wdKor
             * 습도 reh
             */

        }

        Point lastPoint;

        private void panel2_MouseMove(object sender, MouseEventArgs e)
        {
            if(e.Button == MouseButtons.Left)
            {
                this.Left += e.X - lastPoint.X;
                this.Top += e.Y - lastPoint.Y;
            }
        }

        private void panel2_MouseDown(object sender, MouseEventArgs e)
        {
            lastPoint = new Point(e.X, e.Y);
        }

        //private void button1_Click(object sender, EventArgs e)
        //{
        //    for (int i = 8; i <=64; i+=7)
        //    {
        //        richTextBox1.Text += "label" + i.ToString() + ",";
        //    }
        //}
    }
}
```
![image](https://user-images.githubusercontent.com/115389450/235431949-a98821e4-0f41-4c98-aa85-e1278299e7c3.png)
![image](https://user-images.githubusercontent.com/115389450/235431982-7bbd4c3b-4764-49dd-9234-3886c2da8824.png)
![image](https://user-images.githubusercontent.com/115389450/235432008-bd7e2576-805b-4ae4-bd63-9ee10ecf0670.png)
![image](https://user-images.githubusercontent.com/115389450/235432043-7cc422b8-1c92-4d8c-b6e1-35210c350a58.png)
![image](https://user-images.githubusercontent.com/115389450/235432073-10f72d3a-d00c-4610-84dc-5f625963afd0.png)
