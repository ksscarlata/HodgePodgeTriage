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
    
class Patient // : IInjury
    {
        //TRY TO MAKE CLASSES RESPONSIBLE FOR ONLY ONE RESPONSIBILITY
        //FOR EASY UPDATING AND FUTURE CHANGES
        
        #region FIELDS
        private string _patientName;
        private int _patientAge;
        private bool _patientBreathing;
        private string _patientInjury = "'I don't feel any pain there.'";        
        //private bool _walking;
        //private bool _coherent = true;
        //private int _respirations;
        #endregion

        #region CONSTRUCTORS
        public Patient() //default
        {            
            this._patientAge = 0;
            this._patientName = "No patient";
            this._patientBreathing = true;
            this._patientInjury = "No injury.";
        }
        public Patient(string name, int age, bool breathing, string patientInjury) //parameterized
        {               
            this._patientAge = age;
            this._patientName = name;
            this._patientBreathing = breathing;
            this._patientInjury = patientInjury;
        }
        public Patient(Patient p) //copy
        {
            this._patientAge = p.PatientAge;
            this._patientName = p.PatientName;
            this._patientBreathing = p._patientBreathing;
            this._patientInjury = p._patientInjury;
        }
        #endregion

        #region PROPERTIES
        //encapsulate private fields
        public string PatientName { get => _patientName; set => _patientName = value; }
        public int PatientAge { get => _patientAge; set => _patientAge = value; }
        public bool Breathing { get => _patientBreathing; set => _patientBreathing = value; }        
        public string PatientInjury
        {
            get => _patientInjury;
            set
            {
                if (PatientName.Contains("Keith"))
                {
                    _patientInjury = "Ouch! That hurts!!";
                }
            }                
        }
        #endregion

        #region METHODS
        /// <summary>
        /// Makes a list of Patient(), randomizes it, and returns first Patient() in list
        /// </summary>
        public Patient CreatePatientFromList()
        {
            //originally had List<> in Form1.cs but moved here to try to clean up "dirty" code

            List<Patient> patienList = new List<Patient>(); //list of patients
            patienList.Add(new Patient("Keith", 37, false, "Can't breath!"));
            patienList.Add(new Patient("Jerry", 60, true, "Sharp pain in chest!"));
            patienList.Add(new Patient("Cory", 35, true, "women"));
            patienList.Add(new Patient("Jeff", 35, true, "money"));
            patienList.Add(new Patient("Roosevelt", 38, true, "My groin is on fire!"));
            patienList.Add(new Patient("Rosario", 38, true, "nothing at all"));

            Random rnd = new Random();
            patienList = patienList.OrderBy(item => rnd.Next()).ToList(); //sorts patientList randomly

            return patienList.ElementAt(0); //use of first patient in randomized list
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
            CurrentPatient =  CurrentPatient.CreatePatientFromList(); //points CurrentPatient to random list in Patient()

            textBox1.Text = CurrentPatient.PatientName;

            //print CurrentPatient info in textbox2
            textBox2.Text =  
                "Name:\t" + CurrentPatient.PatientName + "\r\n" +
                "Age:\t" + Convert.ToString(CurrentPatient.PatientAge) + "\r\n" +
                "Injury:\t" + CurrentPatient.PatientInjury + "\r\n" +
                "Breathing:\t" + CurrentPatient.Breathing;

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
            if (checkBox1.Checked == false)
            {
                checkBox1.Checked = true;
                CurrentPatient.Breathing = true;
                MessageBox.Show(CurrentPatient.PatientName + " is breathing again! Great job!");
            }
            else
            {
                MessageBox.Show("No reason for this, " + CurrentPatient.PatientName +
                                " is breathing just fine.");
            }
        }
        #endregion

        #region ER BUTTON
        private void ErButton_Click(object sender, EventArgs e)
        {
            textBox1.Text = CurrentPatient.PatientName + " has been sent to the ER.";
            textBox2.Text = String.Empty;
            CurrentPatient = null;
            
        }
        #endregion

        private void button24_Click(object sender, EventArgs e)
        {

        }
    }
