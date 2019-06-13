# test2
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Forms;
using System.IO;

<<<<<<< HEAD
namespace VernamV2
{
    public partial class Form1 : Form
    {
	//Public Values
        string CharacterSet = "~ ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz<>:1234567890!@#$%^&*()_+=-,./;?{}[]\\|\"\'";
        string Key = null;
        string PlainText = null;
        string passcode = null;
        StreamReader sr;
        Random rnd = new Random();

        public Form1()
        {
            InitializeComponent();
            MessageBox.Show("Please Give Correct Path To One Time Pad Before Encrypting or Decrypting!");
            button1.Enabled = false;
            button2.Enabled = false;
            button3.Enabled = true;
            button4.Enabled = true;
        }

	//Button Encrypt
        private void button1_Click(object sender, EventArgs e)
        {
            int Divisor = CharacterSet.Length;
            PlainText = textBox1.Text;
            passcode = "";
            try
            {
                int Passcode = rnd.Next(Key.Length - PlainText.Length);
                passcode = Passcode.ToString();
                if (Convert.ToInt16(passcode) < Key.Length - PlainText.Length)
                {
                    string keysub = Key.Substring(Convert.ToInt16(passcode), PlainText.Length);
                    string CipherText = "";
                    for (int i = 0; i < PlainText.Length; i++)
                    {
                        int sum = CharacterSet.IndexOf(PlainText[i]) + CharacterSet.IndexOf(keysub[i]);
                        sum = sum % Divisor;
                        CipherText = CipherText + CharacterSet.Substring(sum,1);
                    }
                    textBox2.Text = CipherText;
                    MessageBox.Show("Your Passcode is: " + passcode);
                }
                else
                    MessageBox.Show("Passcode must be less than 55,000 - Try Again");

            }
            catch (Exception)
            {
                MessageBox.Show("Invald passcode: Try again");
            }
            
        }

	//Button Decrypt
        private void button2_Click(object sender, EventArgs e)
        {
            int Divisor = CharacterSet.Length;
            string DecodedMessage = "";
            passcode = textBox7.Text;
            string EncryptedText = textBox2.Text;
            try
            {
                if (Convert.ToInt16(passcode) < (Key.Length - EncryptedText.Length))
                {
                    string keysub = Key.Substring(Convert.ToInt16(passcode), EncryptedText.Length);
                    for (int i = 0; i < textBox2.Text.Length; i++)
                    {
                        int difference = CharacterSet.IndexOf(EncryptedText[i]) - CharacterSet.IndexOf(keysub[i]);
                        if (difference < 0)
                        {
                            difference += Divisor;
                        }
                        DecodedMessage = DecodedMessage + CharacterSet.Substring(difference, 1);
                    }
                    textBox3.Text = DecodedMessage;
                }
                else
                    MessageBox.Show("Passcode must be less than 55,000 - Try Again");
            }
            catch (Exception)
            {
                MessageBox.Show("Invald passcode: Try again");
            }
        }

	//Function Create's a One Time Pad.
        public void CreateOneTimePad(string path)
        {
            int x = 0;
            string pad = null;
            for (int i = 0; i < 30000; i++)
            {
                x = rnd.Next(32, 127);
                pad = pad + Convert.ToChar(x);
            }
            using ( StreamWriter sw = new StreamWriter(@path))
            {
                sw.WriteLine(pad);
            }
        }

	// Button Test Path to make sure the program can find the path to the txt file.
        private void button3_Click(object sender, EventArgs e)
        {
            try
            {
                string path = textBox5.Text;
                sr = new StreamReader(@path);
                MessageBox.Show("Good Job! Path Found!");
                button1.Enabled = true;
                button2.Enabled = true;
                button3.Enabled = false;
                button4.Enabled = false;
                Key = sr.ReadLine();
            }
            catch (Exception)
            {
                MessageBox.Show("Incorrect Path -- Try Again");
            }
        }
	// Button Create One Time Pad Given an already created txt file exists and is in the path text box.
        private void button4_Click(object sender, EventArgs e)
        {
            try
            {
                string path = textBox5.Text;
                if (File.Exists(path))
                {
                    CreateOneTimePad(path);
                    button4.Enabled = false;
                }
                else
                    MessageBox.Show("Incorrect Path -- Please Create a Text File and Specify the Path to it in the Text Box Labeled Path");
            }
            catch (Exception)
            {
                MessageBox.Show("Incorrect Path -- Please Create a Text File and Specify the Path to it in the Text Box Labeled Path");
            }
        }
    }
}
