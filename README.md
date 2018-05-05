# HodgePodgeTriage

using System;
using System.Collections.Generic;
using System.Linq;
using System.Threading.Tasks;
using System.Windows.Forms;

namespace HodgePodgeTriage
{
    static class Program
    {
        /// <summary>
        /// The main entry point for the application.
        /// </summary>
        [STAThread]
        static void Main()
        {
            Application.EnableVisualStyles();
            Application.SetCompatibleTextRenderingDefault(false);
            Application.Run(new Form1());
            
        }
    }
}
    
 class Patient 
    {       
        #region FIELDS
        private string _patientName;
        private int _patientAge;                            
        private bool _patientBreathing;
        private bool _patientHeartbeat;
        private bool _patientResponsive;
        private string _patientInjury;
        private bool _patientBleedingProfusely;
        #endregion

        #region CONSTRUCTORS
        public Patient() //default
        {
            this._patientName = "No one";
            this._patientAge = 0;            
            this._patientBreathing = true;
            this._patientHeartbeat = true;
            this._patientResponsive = true;
            this._patientInjury = "No injury.";            
            this._patientBleedingProfusely = false;
        }
        public Patient( string name, int age, bool breathing, bool heartbeat, bool responsive, string injury, bool bleeding ) //parameterized
        {
            this._patientName = name;
            this._patientAge = age;            
            this._patientBreathing = breathing;
            this._patientHeartbeat = heartbeat;
            this._patientResponsive = responsive;
            this._patientInjury = injury;            
            this._patientBleedingProfusely = bleeding;
        }
        public Patient( Patient p ) //copy
        {
            this._patientName = p.PatientName;
            this._patientAge = p.PatientAge;            
            this._patientBreathing = p._patientBreathing;
            this._patientHeartbeat = p._patientHeartbeat;
            this._patientResponsive = p._patientResponsive;
            this._patientInjury = p._patientInjury;            
            this._patientBleedingProfusely = p._patientBleedingProfusely;
        }
        #endregion

        #region PROPERTIES
        //encapsulate private fields with getters and setters
        public string PatientName { get => _patientName; set => _patientName = value; }
        public int PatientAge { get => _patientAge; set => _patientAge = value; }
        public bool Breathing { get => _patientBreathing; set => _patientBreathing = value; }
        public bool Heartbeat { get => _patientHeartbeat; set => _patientHeartbeat = value; }
        public bool Responsive { get => _patientResponsive; set => _patientResponsive = value; }
        public string PatientInjury { get => _patientInjury; set => _patientInjury = value; }       
        public bool Bleeding { get => _patientBleedingProfusely; set => _patientBleedingProfusely = value; }
        #endregion

        #region METHODS
        // One method for least responsibility per class
        /// <summary>
        /// Makes a list of Patient(), randomizes it, and returns first Patient() in list
        /// </summary>
        public Patient CreatePatientFromList()
        {
            //originally had List<> in Form1.cs but moved here to try to clean up "dirty" code
                       
            List<Patient> patientList = new List<Patient>(); //list of patients                                             //SUGGESTED ACTIONS PER PATIENT
            patientList.Add(new Patient("Keith Scarlata", 38, false, true, true, "My neck! I Can't breath!", false));           //CLEAR AIRWAY
            patientList.Add(new Patient("Jerry Koetzle", 60, true, false, true, "I think I'm having a heart attack!", false));  //DEFIBRILATOR
            patientList.Add(new Patient("Cory Small", 35, true, true, true,  "My left knee hurts!", false));                         //WAITING ROOM
            patientList.Add(new Patient("Jeff Christensen", 35, true, true, true, "Give me money!", false));                    //YOU'RE FINE
            patientList.Add(new Patient("Roosevelt Lungren", 38, true, true, true, "My groin is on fire!", true));              //TURNIQUET GROIN :)
            patientList.Add(new Patient("Rosario Dawson", 38, true, true, true, "Absolutely nothing at all :)", false));                  //SPECIAL REACTIONS => YOU'RE FINE
            patientList.Add(new Patient("Danny Dressing", 38, true, true, true, "Ahh! My left hand!!!", true));                           //TOURNIQUET LEFT HAND
            patientList.Add(new Patient("Steve Koppman", 59, true, true, true, "My right shin is killing me!", false));    //WAITING ROOM
            patientList.Add(new Patient("Phoenix Scarlata", 8, true, true, true, "Wait! This is a dog!", false));               //YOU'RE FINE
            patientList.Add(new Patient("Amy Grave", 35, false, false, false, "Wait! This is a dog!", true));

            Random rnd = new Random();
            patientList = patientList.OrderBy(item => rnd.Next()).ToList(); //sorts patientList randomly            
            
            return patientList.ElementAt(0); //use of first patient in randomized list            
        }        
        #endregion
    }   
   
public partial class Form1 : Form
    {
        internal Patient CurrentPatient = new Patient(); //CurrentPatient created for use in Form1
        internal Patient DifferentPatient = new Patient(); //check for different Patient used in NewPatientButton

        public Form1()
        { 
            InitializeComponent();
        }

        private void Form1_Load(object sender, EventArgs e)
        {
        }

        #region NEW PATIENT BUTTON   
        /// <summary>
        /// sets CurrentPatient to CreatePatientFromList() and prints CurrentPatient info to textbox2
        /// </summary>
        private void NewPatientButton_Click(object sender, EventArgs e)
        {
            DifferentPatient = CurrentPatient;

            CurrentPatient = CurrentPatient.CreatePatientFromList(); //retrieves CurrentPatient from random list in Patient()

            while (CurrentPatient.PatientName == DifferentPatient.PatientName) //CurrentPatient different than last used Patient()
            {
                CurrentPatient = CurrentPatient.CreatePatientFromList();
            }

            textBox1.Text = CurrentPatient.PatientName;

            textBox2.Text =
                "Name:\t" + CurrentPatient.PatientName + "\r\n" +
                "Age:\t" + Convert.ToString(CurrentPatient.PatientAge) + "\r\n" +
                "Complaint: \r\n\t" + CurrentPatient.PatientInjury;

            checkBox1.Checked = CurrentPatient.Breathing;
            checkBox2.Checked = CurrentPatient.PatientResponsive;
        }
        #endregion

        #region HEAD BUTTON
        private void HeadButton_Click(object sender, EventArgs e)
        {
            if (CurrentPatient.PatientName.Contains("Rosario"))
            {
                MessageBox.Show("Great smile Rosario!");
            }

            else if (CurrentPatient != null)
            {
                MessageBox.Show(CurrentPatient.PatientName + "'s head is ok.");
            }
        }

        private void HeadButton_MouseEnter(object sender, EventArgs e)
        {
            textBox1.Text = "This is " + CurrentPatient.PatientName + "'s head.";
        }

        private void HeadButton_MouseLeave(object sender, EventArgs e)
        {
            textBox1.Text = null;
        }
        #endregion

        #region NECK BUTTON
        private void NeckButton_Click(object sender, EventArgs e)
        {
            if (CurrentPatient.PatientName.Contains("Rosario"))
            {
                MessageBox.Show("Pretty much perfect.");
            }

            else if (CurrentPatient != null)
            {
                if (CurrentPatient.PatientInjury.Contains("neck"))
                {
                    MessageBox.Show(CurrentPatient.PatientName + "'s neck is injured.");
                }
                else
                {
                    MessageBox.Show(CurrentPatient.PatientName + "'s neck is ok.");
                }
            }
        }

        private void NeckButton_MouseEnter(object sender, EventArgs e)
        {
            textBox1.Text = "This is " + CurrentPatient.PatientName + "'s neck.";
        }

        private void button3_MouseLeave(object sender, EventArgs e)
        {
            textBox1.Text = null;
        }
        #endregion

        #region CHEST BUTTON
        private void ChestButton_Click(object sender, EventArgs e)
        {
            if (CurrentPatient.PatientName.Contains("Rosario"))
            {
                MessageBox.Show("Everything looks great here.");
            }

            else if (CurrentPatient != null)
            {
                MessageBox.Show(CurrentPatient.PatientName + "'s chest is ok.");
            }            
        }

        private void ChestButton_MouseEnter(object sender, EventArgs e)
        {
            textBox1.Text = "This is " + CurrentPatient.PatientName + "'s chest.";
        }

        private void ChestButton_MouseLeave(object sender, EventArgs e)
        {
            textBox1.Text = null;
        }
        #endregion

        #region CLEAR AIRWAY BUTTON        
        private void ClearAirwayButton_Click(object sender, EventArgs e)
        {
            if (CurrentPatient != null)
            {
                if (checkBox1.Checked == false)
                {
                    CurrentPatient.Breathing = true;
                    checkBox1.Checked = true;
                    MessageBox.Show(CurrentPatient.PatientName + " is breathing again! Great job!");
                    CurrentPatient.PatientInjury = "Whew, that's more like it! Thanks!";
                }
                else
                {
                    MessageBox.Show("No reason for this, " + CurrentPatient.PatientName +
                                    " is breathing just fine.");
                }

                textBox2.Text =
                "Name:\t" + CurrentPatient.PatientName + "\r\n" +
                "Age:\t" + Convert.ToString(CurrentPatient.PatientAge) + "\r\n" +
                "Response: \r\n\t" + CurrentPatient.PatientInjury;
            }
        }
        #endregion

        #region DEFIBRILLATOR BUTTON
        private void DefibrillatorButton_Click(object sender, EventArgs e)
        {
            if (CurrentPatient != null)
            {
                if (checkBox2.Checked == false)
                {
                    CurrentPatient.PatientResponsive = true;
                    checkBox2.Checked = true;
                    MessageBox.Show(CurrentPatient.PatientName + " was revived! Great job!");
                    CurrentPatient.PatientInjury = "Whoah, where am I? What happened?";
                }
                else
                {
                    MessageBox.Show("No reason for this, " + CurrentPatient.PatientName +
                                    " is awake and responsive.");
                }

                textBox2.Text =
                "Name:\t" + CurrentPatient.PatientName + "\r\n" +
                "Age:\t" + Convert.ToString(CurrentPatient.PatientAge) + "\r\n" +
                "Response: \r\n\t" + CurrentPatient.PatientInjury;
            }
        }
        #endregion
        
        #region TOURNIQUET BUTTON
        private void TourniquetButton_Click(object sender, EventArgs e)
        {
            if (CurrentPatient != null)
            {
                if (checkBox3.Checked == true)
                {
                    CurrentPatient.Bleeding = false;
                    checkBox3.Checked = false;
                    MessageBox.Show(CurrentPatient.PatientName + " stopped bleeding! Great job!");
                    CurrentPatient.PatientInjury = "That's better, but now what!?";
                }
                else
                {
                    MessageBox.Show("No reason for this, " + CurrentPatient.PatientName +
                                    " isn't bleeding profusely.");
                }

                textBox2.Text =
                "Name:\t" + CurrentPatient.PatientName + "\r\n" +
                "Age:\t" + Convert.ToString(CurrentPatient.PatientAge) + "\r\n" +
                "Response: \r\n\t" + CurrentPatient.PatientInjury;
            }
        }
        #endregion

        #region ER BUTTON
        private void ErButton_Click(object sender, EventArgs e)
        {
            if (textBox2.Text == string.Empty)
            {
                textBox1.Text = null;
                MessageBox.Show("No reason for this.");
            }

            else if (CurrentPatient != null)
            {
                textBox1.Text = CurrentPatient.PatientName + " has been sent to the ER.";
                textBox2.Text = String.Empty;
                CurrentPatient = new Patient();
            }
        }
        #endregion

        #region WAITING ROOM BUTTON
        private void WaitingRoomButton_Click(object sender, EventArgs e)
        {
            if (textBox2.Text == string.Empty)
            {
                textBox1.Text = null;
                MessageBox.Show("No reason for this.");
            }

            else if (CurrentPatient != null)
            {
                textBox1.Text = CurrentPatient.PatientName + " has been sent to the waiting room.";
                textBox2.Text = String.Empty;
                CurrentPatient = new Patient();
            }
        }
        #endregion

        #region YOU'RE JUST FINE BUTTON
        private void YoureJustFineButton_Click(object sender, EventArgs e)
        {
            if (textBox2.Text == string.Empty)
            {
                textBox1.Text = null;
                MessageBox.Show("No reason for this.");
            }

            else if (CurrentPatient != null)
            {
                textBox1.Text = "Get out of here " + CurrentPatient.PatientName + ". You're fine!";
                textBox2.Text = String.Empty;
                CurrentPatient = new Patient();
            }            
        }
        #endregion

        #region DECEASED BUTTON
        private void DeceasedButton_Click(object sender, EventArgs e)
        {
            if (textBox2.Text == string.Empty)
            {
                textBox1.Text = null;
                MessageBox.Show("No reason for this.");
            }

            else if (CurrentPatient != null)
            {
                textBox1.Text = "Time of death for " + CurrentPatient.PatientName + ": " + DateTime.Now;
                textBox2.Text = String.Empty;
                CurrentPatient = new Patient();
            }
        }
        #endregion

        private void checkBox1_CheckedChanged(object sender, EventArgs e)
        {            
        }

        private void checkedListBox1_SelectedIndexChanged(object sender, EventArgs e)
        {
        }

        private void webBrowser1_DocumentCompleted(object sender, WebBrowserDocumentCompletedEventArgs e)
        {
        }

        private void button10_Click(object sender, EventArgs e)
        {
        }

        private void textBox1_TextChanged(object sender, EventArgs e)
        {            
        }

        private void textBox2_TextChanged(object sender, EventArgs e)
        {
        }

        private void button6_Click(object sender, EventArgs e)
        {
        }

        private void textBox2_TextChanged_1(object sender, EventArgs e)
        {
        }

        private void checkBox2_CheckedChanged(object sender, EventArgs e)
        {
        }


    }
