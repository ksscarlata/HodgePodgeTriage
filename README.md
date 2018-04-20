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


class Patient //: IInjury
    {
        #region FIELDS
        private string _patientName;
        private int _patientAge;
        private bool _patientBreathing = true;
        private string _patientInjury = "'I don't feel any pain there.'";        
        private bool _walking;
        private bool _coherent = true;
        private int _respirations;
        #endregion

        #region CONSTRUCTORS
        public Patient() //default
        {
            this._patientAge = 0;
            this._patientName = "No patient.";
            this._patientBreathing = true;
            this._patientInjury = "No injury.";            
        }
        public Patient(string name, int age, bool breathing) //parameterized
        {               
            this._patientAge = age;
            this._patientName = name;
            this._patientBreathing = breathing;
            //this._patientInjury = injury;
        }
        public Patient(Patient p) //copy
        {
            this._patientAge = p.PatientAge;
            this._patientName = p.PatientName;
            this._patientBreathing = p._patientBreathing;
            this._patientInjury = p._patientInjury;
        }       
        # endregion

        #region PROPERTIES
        public string PatientName { get => _patientName; set => _patientName = value; }
        public int PatientAge { get => _patientAge; set => _patientAge = value; }
        public bool Breathing
        {
            get
            {
                if (_patientName == "Keith")
                {                    
                    return _patientBreathing = false;

                }
                else 
                {
                    return _patientBreathing = true;
                }
            }
            set => _patientBreathing = value;
        }
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

        //public string BodyPartInjury { get => PatientInjury; set => }
        //string BodyPartName { get; set; }
        

        #endregion

        #region METHODS

        //public void InitializePatientBodyParts(Patient k)
        //{

        //}

        #endregion
    }
    
    
    class Head : Patient //, IInjury
    {        
        private string _headOwner;
        private string _headInjury;
        protected static string _headName = "head";

        public string HeadOwner { get => _headOwner; set => _headOwner = PatientName; }
        public string HeadInjury { get => _headInjury; set => _headInjury = PatientInjury; }
        public string HeadName { get => _headName; set => _headName = "head"; }


        //private string _owner { get => _Owner; set => _Owner = value; }
        ////public string Name { get; set; }
        //public bool Injured { get; set; }


        //public Head (string owner, string name)
        //{
        //    _owner = owner;
        //    Name = name;
        //    if (name.Contains("keith"))
        //    {
        //        Injured = true;
        //    }
        //    else
        //    {
        //        Injured = false;
        //    }
        //    _owner = owner;
        //    Name = name;


        //}

        //public void BelongsTo()
        //{
        //    Console.WriteLine("this is {0}'s head.", PatientName);
        //}
        //public void IsMissing()
        //{
        //    Console.WriteLine("{0}'s head is gone!", PatientName);
        //}

       
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
        private Patient k = new Patient(); //patient k created for use in Form1
        internal Patient K { get => k; set => k = value; } //incapsulated k for use only in Form1 class

        //
        // Summary:
        //     Initializes new instances of Patient() 
        private void button1_Click(object sender, EventArgs e)
        {            
            List<Patient> patienList = new List<Patient>();
            patienList.Add(new Patient("Keith", 37, false));
            patienList.Add(new Patient("Jerry", 60, true));
            patienList.Add(new Patient("Cory", 35, true));
            patienList.Add(new Patient("Jeff", 35, true));
            patienList.Add(new Patient("Roosevelt", 38, true));
            patienList.Add(new Patient("Rosario", 38, true));

            Random rnd = new Random();
            patienList = patienList.OrderBy(item => rnd.Next()).ToList(); //sorts patientList randomly

            K = patienList.ElementAt(0);
            
            textBox1.Text = K.PatientName;
            
            string ok;
            if (K.Breathing == false)
            {
                ok = "NOT";
            }
            else ok = "";

            textBox2.Text = 
                "Name: " + "\r\t" + K.PatientName + "\r\n" +
                "Age: " + "\r\t" + Convert.ToString(K.PatientAge) + "\r\n" +
                K.PatientName + " is " + ok + " breathing!";
           
            checkBox1.Checked = K.Breathing;            
        }
        #endregion        
       
        private void button10_Click(object sender, EventArgs e)
        {

        }

        #region HEAD BUTTON
        private void button2_Click(object sender, EventArgs e)
        {
            MessageBox.Show(K.PatientName + "'s head is ok.");
        }
        private void button2_MouseEnter(object sender, EventArgs e)
        {
            textBox1.Text = "This is " + K.PatientName + "'s head.";
        }
        private void button2_MouseLeave(object sender, EventArgs e)
        {
            textBox1.Text = "";
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
    }
