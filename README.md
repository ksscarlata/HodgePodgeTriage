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
        private string _patientInjury = "I don't feel any pain there.";
        #endregion

        #region CONSTRUCTORS
        public Patient() //default
        {            
            this._patientAge = 0;
            this._patientName = "No patient";
            this._patientBreathing = true;
            this._patientInjury = "No injury.";
        }
        public Patient( string name, int age, bool breathing, string patientInjury ) //parameterized
        {               
            this._patientAge = age;
            this._patientName = name;
            this._patientBreathing = breathing;
            this._patientInjury = patientInjury;
        }
        public Patient( Patient p ) //copy
        {
            this._patientAge = p.PatientAge;
            this._patientName = p.PatientName;
            this._patientBreathing = p._patientBreathing;
            this._patientInjury = p._patientInjury;
        }
        #endregion

        #region PROPERTIES
        //encapsulate private fields with getters and setters
        public string PatientName { get => _patientName; set => _patientName = value; }
        public int PatientAge { get => _patientAge; set => _patientAge = value; }
        public bool Breathing { get => _patientBreathing; set => _patientBreathing = value; }        
        public string PatientInjury
        {
            get => _patientInjury;
            set => _patientInjury = value;
            //TRIED SETTING THIS BELOW BUT THEN COULDN'T SET IT WITH ANYTHING BUT WHAT'S TYPED HERE IN SETTER
            //{
            //    if (PatientName.Contains("Keith"))
            //    {
            //        _patientInjury = "My neck is still sore, but I'll live.";
            //    }
            //}                
        }      
        #endregion

        #region METHODS
        /// <summary>
        /// Makes a list of Patient(), randomizes it, and returns first Patient() in list
        /// </summary>
        public Patient CreatePatientFromList()
        {
            //originally had List<> in Form1.cs but moved here to try to clean up "dirty" code

            List<Patient> patientList = new List<Patient>(); //list of patients
            patientList.Add(new Patient("Keith Scarlata", 38, false, "My neck! I Can't breath!"));
            patientList.Add(new Patient("Jerry Koetzle", 60, true, "Sharp pain in my chest!"));
            patientList.Add(new Patient("Cory Small", 35, true, "My knee hurts!"));
            patientList.Add(new Patient("Jeff Christensen", 35, true, "Give me money!"));
            patientList.Add(new Patient("Roosevelt Lungren", 38, true, "My groin is on fire!"));
            patientList.Add(new Patient("Rosario Dawson", 38, true, "Absolutely nothing at all :)"));
            patientList.Add(new Patient("Danny Dressing", 38, true, "Ahh! My left hand!!"));
            patientList.Add(new Patient("Steve Koppman", 59, true, "My right lower leg is killing me!"));
            patientList.Add(new Patient("Phoenix Scarlata", 8, true, "Wait! This is a dog!"));

            Random rnd = new Random();
            patientList = patientList.OrderBy(item => rnd.Next()).ToList(); //sorts patientList randomly            
            
            return patientList.ElementAt(0); //use of first patient in randomized list            
        }
        #endregion
    }
    
    
    public interface IInjury
    {
        //properties
        string BodyPartInjury { get; set; }
        string BodyPartName { get; set; }
        //bool BodyPart_IsInjured { get; set; }

        //methods NOT implemented (to be defined inside class)

        /// <summary>
        /// Should set injury to body part 
        /// </summary>
        //void BelongsTo();
        //void IsMissing();
        

    } 
    
    
    public partial class Form1 : Form
    {
        internal Patient CurrentPatient = new Patient(); //patient k created for use in Form1
        internal Patient DifferentPatient = new Patient(); //check for different Patient used in NewPatientButton

        public Form1()
        {
            InitializeComponent();
        }

        private void Form1_Load(object sender, EventArgs e)
        {

        }

        private void checkBox1_CheckedChanged(object sender, EventArgs e)
        {
            
        }

        private void checkedListBox1_SelectedIndexChanged(object sender, EventArgs e)
        {

        }

        private void webBrowser1_DocumentCompleted(object sender, WebBrowserDocumentCompletedEventArgs e)
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
                "\t\tComplaint:\r\n\t" + CurrentPatient.PatientInjury;
                
                //+ "\r\n" +
                //"Breathing:\t" + CurrentPatient.Breathing;

            checkBox1.Checked = CurrentPatient.Breathing;            
        }
        #endregion
        
        private void button10_Click(object sender, EventArgs e)
        {

        }

        #region HEAD BUTTON
        private void HeadButton_Click(object sender, EventArgs e)
        {
            MessageBox.Show(CurrentPatient.PatientName + "'s head is ok.");
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

        private void textBox1_TextChanged(object sender, EventArgs e)
        {
            
        }

        private void button3_Click(object sender, EventArgs e)
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
                    "Complaint:\t" + CurrentPatient.PatientInjury + "\r\n" +
                    "Breathing:\t" + CurrentPatient.Breathing;
            }           
        }
        #endregion

        #region ER BUTTON
        private void ErButton_Click(object sender, EventArgs e)
        {
            if (CurrentPatient != null)
            {
                textBox1.Text = CurrentPatient.PatientName + " has been sent to the ER.";
                textBox2.Text = String.Empty;
                CurrentPatient = new Patient();
            }            
        }
        #endregion

        private void button24_Click(object sender, EventArgs e)
        {

        }
    }
