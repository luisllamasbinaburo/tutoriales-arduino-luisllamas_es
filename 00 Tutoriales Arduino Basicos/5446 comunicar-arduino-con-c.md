/***************************************************
Códigos de ejemplo de los tutoriales de www.luisllamas.es
Enlace entrada: https://www.luisllamas.es/comunicar-arduino-con-c/
Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa
****************************************************/

using System;
using System.Windows.Forms;

namespace BasicSerialPort
{
    public partial class frmMain : Form
    {
        System.IO.Ports.SerialPort ArduinoPort;

        public frmMain()
        {
            InitializeComponent();

            //crear Serial Port
            ArduinoPort = new System.IO.Ports.SerialPort();
            ArduinoPort.PortName = "COM4";  //sustituir por vuestro 
            ArduinoPort.BaudRate = 9600;
            ArduinoPort.Open();

            //vincular eventos
            this.FormClosing += FrmMain_FormClosing;
            this.btOFF.Click += BtOFF_Click;
            this.btON.Click += BtON_Click;
        }

        private void FrmMain_FormClosing(object sender, FormClosingEventArgs e)
        {
            //cerrar puerto
            if (ArduinoPort.IsOpen) ArduinoPort.Close();
        }

        private void BtOFF_Click(object sender, EventArgs e)
        {
            ArduinoPort.Write("a");
        }

        private void BtON_Click(object sender, EventArgs e)
        {
            ArduinoPort.Write("b");
        }
    }
}



const int pinLED = 13;

void setup()
{
	Serial.begin(9600);
	pinMode(pinLED, OUTPUT);
}

void loop() {
	if (Serial.available() > 0)
	{
		int option = Serial.read();
		if (option == 'a')
		{
			digitalWrite(pinLED, LOW);		
		}
		if (option == 'b')
		{
			digitalWrite(pinLED, HIGH);
		}
	}
}


using System;
using System.Windows.Forms;

namespace BasicSerialPort
{
    public partial class frmMain : Form
    {
        System.IO.Ports.SerialPort ArduinoPort;

        public frmMain()
        {
            InitializeComponent();

            //crear Serial Port
            ArduinoPort = new System.IO.Ports.SerialPort();
            ArduinoPort.PortName = "COM4"; //sustituir por vuestro 
            ArduinoPort.BaudRate = 9600;
            ArduinoPort.Open();

            //vincular eventos
            this.FormClosing += FrmMain_FormClosing;
            this.btSend.Click += BtSend_Click;
        }

        private void FrmMain_FormClosing(object sender, FormClosingEventArgs e)
        {
            //cerrar puerto
            if(ArduinoPort.IsOpen) ArduinoPort.Close();
        }

        private void BtSend_Click(object sender, EventArgs e)
        {
            //convertir texto de Textbox en integer
            int value;
            bool rst = Int32.TryParse(txNumber.Text, out value);

            //si la conversion es válida, y número entre 0 y 9, enviar el número como texto
            if (rst == true && value > 0 && value < 9 )
            {
                ArduinoPort.Write(value.ToString());
            }
        }
    }
}


const int pinLED = 13;

void setup() 
{
	Serial.begin(9600);
	pinMode(pinLED, OUTPUT);
}

void loop()
{
	if (Serial.available()>0) 
	{
		char option = Serial.read();
		if (option >= '1' && option <= '9')
		{
			option -= '0';
			for (int i = 0;i<option;i++) {="" digitalwrite(pinled,="" high);="" delay(100);="" digitalwrite(pinled,="" low);="" delay(200);="" }="" }="" }="" }=""></option;i++)>