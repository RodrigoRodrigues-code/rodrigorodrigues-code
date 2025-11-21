using System;

namespace StudentGradesManager
{
    public class Student
    {
        public string Name { get; private set; }
        public string Id { get; private set; }

        // Cada disciplina terá um nome e um conjunto de notas
        private string[] subjects = new string[0];
        private double[][] gradesBySubject = new double[0][];

        public Student(string name, string id)
        {
            Name = name;
            Id = id;
        }

        // Adiciona uma nota a uma disciplina
        public void AddGrade(string subject, double grade)
        {
            int index = FindSubject(subject);

            if (index == -1)
            {
                // Criar nova disciplina
                int newSize = subjects.Length + 1;
                Array.Resize(ref subjects, newSize);
                Array.Resize(ref gradesBySubject, newSize);

                subjects[newSize - 1] = subject;
                gradesBySubject[newSize - 1] = new double[] { grade };
            }
            else
            {
                // Adicionar nota à disciplina existente
                int gradeCount = gradesBySubject[index].Length;
                Array.Resize(ref gradesBySubject[index], gradeCount + 1);
                gradesBySubject[index][gradeCount] = grade;
            }
        }

        // Calcula a média de uma disciplina
        public double CalculateSubjectAverage(string subject)
        {
            int index = FindSubject(subject);
            if (index == -1) return 0.0;

            double sum = 0.0;
            double[] grades = gradesBySubject[index];
            for (int i = 0; i < grades.Length; i++)
            {
                sum += grades[i];
            }
            return grades.Length > 0 ? sum / grades.Length : 0.0;
        }

        // Calcula a média geral do aluno
        public double CalculateOverallAverage()
        {
            double sum = 0.0;
            int totalGrades = 0;

            for (int i = 0; i < gradesBySubject.Length; i++)
            {
                for (int j = 0; j < gradesBySubject[i].Length; j++)
                {
                    sum += gradesBySubject[i][j];
                    totalGrades++;
                }
            }

            return totalGrades > 0 ? sum / totalGrades : 0.0;
        }

        // Exibe os registros do aluno
        public void ShowRecords()
        {
            Console.WriteLine($"Student: {Name} | ID: {Id}");

            if (subjects.Length == 0)
            {
                Console.WriteLine("  No grades recorded.");
                return;
            }

            for (int i = 0; i < subjects.Length; i++)
            {
                string subject = subjects[i];
                double[] grades = gradesBySubject[i];
                string gradesText = grades.Length > 0 ? string.Join(", ", grades) : "No grades";
                Console.WriteLine($"  Subject: {subject} | Grades: {gradesText}");
            }

            Console.WriteLine($"  Overall average: {CalculateOverallAverage():0.00}");
        }

        // Busca disciplina pelo nome
        private int FindSubject(string subject)
        {
            for (int i = 0; i < subjects.Length; i++)
            {
                if (string.Equals(subjects[i], subject, StringComparison.OrdinalIgnoreCase))
                {
                    return i;
                }
            }
            return -1;
        }
    }

    class Program
    {
        // Lista de alunos usando apenas arrays
        private static Student[] students = new Student[0];

        static void Main(string[] args)
        {
            while (true)
            {
                ShowMenu();
                Console.Write("Choose an option: ");
                string option = Console.ReadLine();

                switch (option)
                {
                    case "1":
                        AddStudent();
                        break;
                    case "2":
                        AddGrade();
                        break;
                    case "3":
                        ShowAllRecords();
                        break;
                    case "4":
                        ShowStudentAverages();
                        break;
                    case "5":
                        Console.WriteLine("Exiting... Goodbye!");
                        return;
                    default:
                        Console.WriteLine("Invalid option. Try again.");
                        break;
                }

                Console.WriteLine();
            }
        }

        // Exibe o menu principal
        private static void ShowMenu()
        {
            Console.WriteLine("===== STUDENT GRADES MANAGER =====");
            Console.WriteLine("1 - Add student");
            Console.WriteLine("2 - Add grade to a student");
            Console.WriteLine("3 - Show all student records");
            Console.WriteLine("4 - Show averages of a student");
            Console.WriteLine("5 - Exit");
        }

        // Adiciona um novo aluno
        private static void AddStudent()
        {
            Console.Write("Student name: ");
            string name = Console.ReadLine()?.Trim();

            Console.Write("Student ID: ");
            string id = Console.ReadLine()?.Trim();

            if (string.IsNullOrWhiteSpace(name) || string.IsNullOrWhiteSpace(id))
            {
                Console.WriteLine("Name and ID cannot be empty.");
                return;
            }

            if (FindStudentById(id) != null)
            {
                Console.WriteLine("A student with this ID already exists.");
                return;
            }

            int newSize = students.Length + 1;
            Array.Resize(ref students, newSize);
            students[newSize - 1] = new Student(name, id);

            Console.WriteLine("Student added successfully.");
        }

        // Adiciona uma nota a um aluno
        private static void AddGrade()
        {
            Console.Write("Enter student ID: ");
            string id = Console.ReadLine()?.Trim();

            var student = FindStudentById(id);
            if (student == null)
            {
                Console.WriteLine("Student not found. Check the ID.");
                return;
            }

            Console.Write("Subject name: ");
            string subject = Console.ReadLine()?.Trim();
            if (string.IsNullOrWhiteSpace(subject))
            {
                Console.WriteLine("Subject cannot be empty.");
                return;
            }

            Console.Write("Grade (0 to 10): ");
            string gradeText = Console.ReadLine();
            if (!double.TryParse(gradeText, out double grade))
            {
                Console.WriteLine("Invalid grade. Use numbers (e.g., 7.5).");
                return;
            }

            if (grade < 0.0 || grade > 10.0)
            {
                Console.WriteLine("Grade must be between 0 and 10.");
                return;
            }

            student.AddGrade(subject, grade);
            Console.WriteLine("Grade added successfully.");
        }

        // Exibe todos os registros dos alunos
        private static void ShowAllRecords()
        {
            if (students.Length == 0)
            {
                Console.WriteLine("No students registered.");
                return;
            }

            Console.WriteLine("===== STUDENT RECORDS =====");
            for (int i = 0; i < students.Length; i++)
            {
                students[i].ShowRecords();
            }
        }

        // Exibe médias de um aluno específico
        private static void ShowStudentAverages()
        {
            Console.Write("Enter student ID: ");
            string id = Console.ReadLine()?.Trim();

            var student = FindStudentById(id);
            if (student == null)
            {
                Console.WriteLine("Student not found. Check the ID.");
                return;
            }

            Console.WriteLine($"Student: {student.Name} | ID: {student.Id}");
            student.ShowRecords();
        }

        // Busca aluno pelo RG
        private static Student FindStudentById(string id)
        {
            if (string.IsNullOrWhiteSpace(id)) return null;

            for (int i = 0; i < students.Length; i++)
            {
                if (string.Equals(students[i].Id, id, StringComparison.OrdinalIgnoreCase))
                {
                    return students[i];
                }
            }

            return null;
        }
    }
}
