# Combat-Sports-Scoring

using ConsoleTables;
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;


namespace Combat_Sports
{
    interface FileHandling
    {
        List<Fighters> ReadFromFile(string fileName, string sport);
    }

    interface FightStore
    {
        List<Fighters> WriteToFile(string fileName, string folder, string[] txt);
    }
    interface DeleteRow
    {
        List<Fighters> DeleteRow(int RowToDelete, string fileName, string sport, int rowCount);
    }
    public class Filethis : FileHandling, FightStore, DeleteRow
    {

        public Filethis(){}

        public List<Fighters> ReadFromFile(string fileName, string sport)
        {
            List <Fighters> res = new List <Fighters>();

            Console.WriteLine("--------------- Data from File ---------------");

            using (var reader = new StreamReader(fileName))
            {
                var table = new ConsoleTable("","Name", "Weight");
                int row = 1;
                while (!reader.EndOfStream)
                {
                    var line = reader.ReadLine();
                    var values = line.Split(',');
                    if(values.Length > 1)
                    {
                        table.AddRow(row++, values[0], values[1]);
                        res.Add(new Fighters(values[0], Int32.Parse(values[1]), sport));
                    }
                }
                table.Write();
            }
            return res;
        }
        public List<Fighters> WriteToFile(string fileName, string folder, string[] txt)
        {
          List<Fighters> write = new List<Fighters>();

            string fullPath = folder + fileName;
            // This method automatically opens the file, writes to it, and closes file
            File.WriteAllLines(fullPath, txt);
            // Read a file
            string readText = File.ReadAllText(fullPath);
            Console.ForegroundColor = ConsoleColor.Yellow;
            Console.WriteLine(readText);
            Console.ResetColor();
            return write;
        }
        public List<Fighters> DeleteRow(int RowToDelete, string fileName, string sport, int rowCount)
        {

            Console.WriteLine("rowCount : " + rowCount);

            List<Fighters> res = new List<Fighters>();
           string[] newtxt = new string[rowCount - 1];
            using (var reader = new StreamReader(fileName))
            {
                var table = new ConsoleTable("", "Name", "Weight");
                int oldRow = 1;
                int newRow = 1;
                while (!reader.EndOfStream)
                {
                    if(oldRow == RowToDelete)
                    {
                        reader.ReadLine();
                    }
                    else
                    {
                        var line = reader.ReadLine();
                        var values = line.Split(',');
                        if(values.Length > 1)
                        {
                            table.AddRow(newRow++, values[0], values[1]);
                            res.Add(new Fighters(values[0], Int32.Parse(values[1]), sport));
                            newtxt[newRow - 2] = values[0] + "," + values[1];
                        }                      
                    }
                    oldRow++;
                }
                Console.ForegroundColor = ConsoleColor.Yellow;
                table.Write();
                reader.ReadToEnd();
                Console.ResetColor();
            }
            string writeFile = "./Fighterdata.txt";
            WriteToFile("", writeFile, newtxt); 
            return res;
        }
        public List<Fighters> AddRow(string Name, int weight, string fileName, string sport, int rowCount)
        {

            Console.WriteLine("rowCount : " + rowCount);

            List<Fighters> res = new List<Fighters>();
            string[] newtxt = new string[rowCount + 1];
            using (var reader = new StreamReader(fileName))
            {
                var table = new ConsoleTable("", "Name", "Weight");
                int newRow = 1;
                while (!reader.EndOfStream)
                {
                    var line = reader.ReadLine();
                    var values = line.Split(',');
                    if (values.Length > 1)
                    {                     
                        table.AddRow(newRow, values[0], values[1]);
                        res.Add(new Fighters(values[0], Int32.Parse(values[1]), sport));
                        newtxt[newRow - 1] = values[0] + "," + values[1];
                        newRow++;
                    }
                   
                }

                newtxt[newRow-1] = Name + "," + weight;
                table.AddRow(newRow, Name, weight);
                res.Add(new Fighters(Name, weight, sport));

                table.Write();
                reader.ReadToEnd();
            }
            string writeFile = "./Fighterdata.txt";
            WriteToFile("", writeFile, newtxt);
            return res;
        }
    }
    

    public abstract class CombatSport
    {
        public abstract void DisplayRulesAndRegulations();
        public abstract string GetName();
    }

    class Kickboxing : CombatSport
    {
        public override void DisplayRulesAndRegulations()
        {
            Console.ForegroundColor = ConsoleColor.Yellow;
            Console.WriteLine("<=========================================<Rules and Regulations for Kickboxing>=========================================>");
            Console.ResetColor();
            // Display Kickboxing-specific rules and regulations
            Console.ForegroundColor = ConsoleColor.Cyan;
            Console.WriteLine("1. Head butts.\r\n2. Groin strikes.\r\n3. Thrusting or Linear kicks directed at the knee joint\r\n4. Striking the back of the head or the spine the tailbone\r\n5. Attacks to the throat.");
            Console.ResetColor();
        }

        public override string GetName()
        {
            return "Kickboxing";
        }
    }

    class Boxing : CombatSport
    {
        public override void DisplayRulesAndRegulations()
        {
            Console.ForegroundColor = ConsoleColor.Yellow;
            Console.WriteLine("<=========================================<Rules and Regulations for Boxing>=========================================>");
            Console.ResetColor();
            // Display Boxing-specific rules and regulations
            Console.ForegroundColor = ConsoleColor.Cyan;
            Console.WriteLine("1. Hitting below the navel or behind the ear.\r\n\r\n2. Hitting an opponent who is down or is getting up after being down.\r\n\r\n3. Holding an opponent with one hand and hitting with the other.\r\n\r\n4. Holding or deliberately maintaining a clinch.\r\n\r\n5. Wrestling or kicking.\r\n\r\n6. Striking an opponent who is helpless as a result of previous blows and so supported by the ropes that he does not fall after being instructed by the referee to a neutral corner.\r\n\r\n7. Butting with the head or shoulder or using the knee.\r\n\r\n8. Hitting with the open glove, the butt of the hand, the wrist or the elbow, and all backhand blows.");
            Console.ResetColor();
        }

        public override string GetName()
        {
            return "Boxing";
        }
    }

    class MMA : CombatSport
    {
        public override void DisplayRulesAndRegulations()
        {
            Console.ForegroundColor = ConsoleColor.Yellow;
            Console.WriteLine("<=========================================<Rules and Regulations for MMA>=========================================>");
            Console.ResetColor();
            // Display MMA-specific rules and regulations
            Console.ForegroundColor = ConsoleColor.Cyan;
            Console.WriteLine("1. Strikes to the spine or back of the head or anything behind the ears (see Rabbit punch) Throat strikes of any kind, including, without limitation, grabbing the trachea.\r\n\r\n2. Fingers outstretched towards opponent's face/eyes. Clawing, pinching, twisting the flesh.");
            Console.ResetColor();
        }

        public override string GetName()
        {
            return "MMA";
        }
    }

    public class Fighters
    {
        string name;
        string weightclass;
        int weight;
        string eventSport;
        public Fighters(string name, int weight, string eventSport)
        {
            this.name = name;
            this.weight = weight;
            this.eventSport = eventSport;
        }
        public int Weight
        {
            get { return weight; }
            set { weight = value;}
        }
        public string WeightClass
        {
            get { return weightclass; }
            set{ weightclass = value; }
        }
        public string Name
        {
            get { return name; }
            set { name = value;}
        }
        public string EventSport
        {
            get { return eventSport; }
            set { eventSport = value; }
        }
    }

    public class Weightclass
    {
        public string name { get; set; }
        public int upperbound { get; set; }
        public int lowerbound { get; set; }
        public Weightclass(string name, int upperbound, int lowerbound)
        {
            this.name = name;
            this.upperbound = upperbound;
            this.lowerbound = lowerbound;
        }
    }

    public class WeightClassProcessor
    {
        public static int fighterPair = 1;

        public List<Matches> ProcessWeightClass(List<Fighters> fighters, List<Weightclass> weightClasses)
        {
            List<Fighters> Flyweight = new List<Fighters>();
            List<Fighters> Bantamweight = new List<Fighters>();
            List<Fighters> Featherweight = new List<Fighters>();
            List<Fighters> Lightweight = new List<Fighters>();
            List<Fighters> Welterweight = new List<Fighters>();
            List<Fighters> Middleweight = new List<Fighters>();
            List<Fighters> LightHeavyweight = new List<Fighters>();
            List<Fighters> Heavyweight = new List<Fighters>();

            foreach (var player in fighters)
            {
                int playerWeight = player.Weight;

                foreach (var category in weightClasses)
                {
                    if (playerWeight >= category.lowerbound && playerWeight <= category.upperbound)
                    {
                        string categoryName = category.name;
                        switch (categoryName)
                        {
                            case "Flyweight":
                                Flyweight.Add(player);
                                player.WeightClass = "Flyweight";
                                break;
                            case "Bantamweight":
                                Bantamweight.Add(player);
                                player.WeightClass = "Bantamweight";
                                break;
                            case "Featherweight":
                                Featherweight.Add(player);
                                player.WeightClass = "Featherweight";
                                break;
                            case "Lightweight":
                                Lightweight.Add(player);
                                player.WeightClass = "Lightweight";
                                break;
                            case "Welterweight":
                                Welterweight.Add(player);
                                player.WeightClass = "Welterweight";
                                break;
                            case "Middleweight":
                                Middleweight.Add(player);
                                player.WeightClass = "Middleweight";
                                break;
                            case "LightHeavyweight":
                                LightHeavyweight.Add(player);
                                player.WeightClass = "LightHeavyweight";
                                break;
                            case "Heavyweight":
                                Heavyweight.Add(player);
                                player.WeightClass = "Heavyweight";
                                break;
                            default:
                                Console.WriteLine("Invalid");
                                break;
                        }
                    }
                }        
            }
            List<Matches> finalMatches = new List<Matches>();
            
            if (Flyweight.Count > 0)
            {
                Console.ForegroundColor = ConsoleColor.Cyan;
                Console.WriteLine("\n\n---------- Flyweight Matches ----------\n");
                Console.ResetColor();
                finalMatches.AddRange(Pairing(Flyweight, "Flyweight"));
            }
            if (Bantamweight.Count > 0)
            {
                Console.ForegroundColor = ConsoleColor.Cyan;
                Console.WriteLine("\n\n---------- Bantamweight Matches ----------\n");
                Console.ResetColor();
                finalMatches.AddRange(Pairing(Bantamweight, "Bantamweight"));
            }
            if (Featherweight.Count > 0)
            {
                Console.ForegroundColor = ConsoleColor.Cyan;
                Console.WriteLine("\n\n---------- Featherweight Matches ----------\n");
                Console.ResetColor();
                finalMatches.AddRange(Pairing(Featherweight, "Featherweight"));
            }
            if (Lightweight.Count > 0)
            {
                Console.ForegroundColor = ConsoleColor.Cyan;
                Console.WriteLine("\n\n---------- Lightweight Matches ----------\n");
                Console.ResetColor();
                finalMatches.AddRange(Pairing(Lightweight, "Lightweight"));
            }
            if (Welterweight.Count > 0)
            {
                Console.ForegroundColor = ConsoleColor.Cyan;
                Console.WriteLine("\n\n---------- Welterweight Matches ----------\n");
                Console.ResetColor();
                finalMatches.AddRange(Pairing(Welterweight, "Welterweight"));
            }
            if (Middleweight.Count > 0)
            {
                Console.ForegroundColor = ConsoleColor.Cyan;
                Console.WriteLine("\n\n---------- Middleweight Matches ----------\n");
                Console.ResetColor();
                finalMatches.AddRange(Pairing(Middleweight, "Middleweight"));
            }
            if (LightHeavyweight.Count > 0)
            {
                Console.ForegroundColor = ConsoleColor.Cyan;
                Console.WriteLine("\n\n---------- LightHeavyweight Matches ----------\n");
                Console.ResetColor();
                finalMatches.AddRange(Pairing(LightHeavyweight, "LightHeavyweight"));
            }
            if (Heavyweight.Count > 0)
            {
                Console.ForegroundColor = ConsoleColor.Cyan;
                Console.WriteLine("\n\n---------- Heavyweight Matches ----------\n");
                Console.ResetColor();
                finalMatches.AddRange(Pairing(Heavyweight, "Heavyweight"));
            }
            return finalMatches;
        }
        public List<Matches> Pairing(List<Fighters> list, string category)
        {
            List<Matches> matches = new List<Matches>();

            Random rng = new Random();
            int n = list.Count;

            while (n > 1)
            {
                n--;
                int k = rng.Next(n + 1);
                Fighters value = list[k];
                list[k] = list[n];
                list[n] = value;
            }
            int i = 1;
            for (int j = 0; j < list.Count - 1; j++)
            {
                Fighters f1 = list[j];
                Fighters f2 = list[j + 1];
                Console.ForegroundColor = ConsoleColor.Green;
                Console.WriteLine("[" + fighterPair++ + "] " + f1.Name + " VS " + f2.Name);
                Console.ResetColor();
                Matches a = new Matches(category, i, f1, f2);
                matches.Add(a);
                j++;
                i++;
            }
            return matches;
        }
    }

    public class Matches
    {
        public string weightClass { get; set; }
        public int pairNum { get; set; }
        public Fighters fighterOne;
        public Fighters fighterTwo;
        public Matches(string weightClass, int pairNum, Fighters fighterOne, Fighters fighterTwo)
        {
            this.weightClass = weightClass;
            this.pairNum = pairNum; 
            this.fighterOne = fighterOne;
            this.fighterTwo = fighterTwo;
        }
        public Fighters FighterOne
        {
            get { return fighterOne; }
            set { fighterOne = value; }
        }
        public Fighters FighterTwo
        {
            get { return fighterTwo; }
            set { fighterTwo = value; }
        }
    }
    internal class Program
    {
        static void Main(string[] args)
        {
            try
            {
                char Y = 'Y';           
                do
                {
                    List<Fighters> fighters = new List<Fighters>();
                    List<Weightclass> weightclass = InitiateWeightClass();
                    CombatSport selectedSport = null;
                    string matchResultTxt;
                    string file = "./Fightertable.txt";
                    Filethis a = new Filethis();
                    matchResultTxt = "";
                    // Main Menu
                    Console.ForegroundColor = ConsoleColor.Yellow;
                    Console.WriteLine("[======<======<======<======<======{Martial Arts Selection}======>======>======>======>======]");
                    Console.ResetColor();
                    selectedSport = DisplayMainMenu();
                    Console.ForegroundColor = ConsoleColor.Yellow;
                    Console.WriteLine("[Y]{Do you want to load the data from file?}\n[N]{Skip}");
                    Console.ResetColor();
                    char loadChoice;
                    do
                    {
                        loadChoice = char.ToUpper(Console.ReadKey().KeyChar);
                        if (loadChoice != 'Y' && loadChoice != 'N')
                        {
                            //Console.Clear();
                            Console.ForegroundColor = ConsoleColor.Cyan;
                            Console.WriteLine("Invalid choice. Press [Y] to load from file or [N] to skip.");
                            Console.ResetColor();
                        }
                    } while (loadChoice != 'Y' && loadChoice != 'N');
                    Console.Clear();
                    if (loadChoice == 'Y')
                    {
                        Console.ForegroundColor = ConsoleColor.Cyan;
                        Console.WriteLine($"<=====<=====<=====<=====||{selectedSport.GetType().Name} Associates||=====>=====>=====>=====>");
                        Console.ResetColor();
                        try
                        {    
                            fighters = a.ReadFromFile(file, selectedSport.GetName());
                        }
                        catch (Exception ex)
                        {
                            Console.WriteLine($"An error occurred: {ex.Message}");
                        }
                        Console.ForegroundColor = ConsoleColor.Cyan;
                        Console.WriteLine("[A]{Do you want to add a fighter?}\n[D]{Do you want to remove a fighter}\n[N]{None}");
                        char tableChoice;
                        Console.ResetColor();
                        do
                        {
                            tableChoice = char.ToUpper(Console.ReadKey().KeyChar);                         
                            if (tableChoice == 'D')
                            {
                                Console.Clear();
                                Console.ForegroundColor = ConsoleColor.Yellow;
                                Console.WriteLine("Current Data Table:");                              
                                List<Fighters> currentData = a.ReadFromFile(file, selectedSport.GetName());
                                Console.ResetColor();
                                int RowToDelete = 0;                                                             
                                do
                                {
                                    Console.ForegroundColor = ConsoleColor.Green;
                                    Console.Write("The number of row to be removed: ");
                                    Console.ResetColor();
                                    string rowDel = Console.ReadLine();
                                    try
                                    {
                                        RowToDelete = int.Parse(rowDel);
                                        Console.WriteLine();
                                    }
                                    catch (FormatException)
                                    {
                                        Console.WriteLine($"Invalid");
                                    }
                                } while (RowToDelete == 0);
                                Console.ForegroundColor = ConsoleColor.Yellow;
                                fighters = a.DeleteRow(RowToDelete, file, selectedSport.GetName(), fighters.Count);
                                Console.ResetColor();
                                Console.ForegroundColor = ConsoleColor.Green;
                                Console.WriteLine("[A]{Do you want to add a fighter?}\n[D]{Do you want to remove a fighter}\n[N]{None}");
                                Console.ResetColor();
                            }                         
                            else if (tableChoice == 'A')
                            {
                                //Console.Clear();
                                string fighterNameAdd = "";
                                int weightToAdd = 0;
                                Console.Clear();
                                Console.ForegroundColor = ConsoleColor.Yellow;
                                Console.WriteLine("<<<<<<<<<<<<<<<<Adding Participants>>>>>>>>>>>>>>>>");
                                Console.ResetColor();
                                Console.ForegroundColor = ConsoleColor.Cyan;
                                Console.WriteLine("\n<WeightClass Guide>");
                                Console.WriteLine(">Flyweight(120 - 126)");
                                Console.WriteLine(">Bantamweight(127 - 136)");
                                Console.WriteLine(">Featherweight(146, 137)");
                                Console.WriteLine(">Lightweight(147 - 156)");
                                Console.WriteLine(">Welterweight(157 - 171)");
                                Console.WriteLine(">Middleweight(172 - 186)");
                                Console.WriteLine(">LightHeavyweight(187 - 206)");
                                Console.WriteLine(">Heavyweight(207 - 265)");
                                Console.ResetColor();
                                do
                                {
                                    Console.ForegroundColor = ConsoleColor.Green;
                                    Console.Write("\nEnter the name of the fighter: ");
                                    string FighterAdd = Console.ReadLine();
                                    Console.ResetColor();
                                    try
                                    {
                                        int.Parse(FighterAdd);
                                        Console.WriteLine($"Invalid Name");
                                    }
                                    catch (FormatException)
                                    {
                                        fighterNameAdd = FighterAdd;
                                    }
                                } while (string.IsNullOrEmpty(fighterNameAdd) || fighterNameAdd.Trim() == "");
                                
                                do
                                {
                                    Console.ForegroundColor = ConsoleColor.Green;
                                    Console.Write("Enter the weight of the fighter: ");
                                    string WeightAdd = Console.ReadLine();
                                    Console.ResetColor();
                                    try
                                    {
                                        weightToAdd = int.Parse(WeightAdd);
                                        Console.WriteLine();
                                    }
                                    catch (FormatException)
                                    {
                                        Console.WriteLine($"Invalid Weight");
                                    }
                                } while (weightToAdd == 0);
                                Console.Clear();
                                Console.ForegroundColor = ConsoleColor.Yellow;
                                fighters = a.AddRow(fighterNameAdd, weightToAdd, file, selectedSport.GetName(), fighters.Count);
                                Console.ResetColor();
                                Console.ForegroundColor = ConsoleColor.Cyan;
                                Console.WriteLine("[A]{Do you want to add a fighter?}\n[D]{Do you want to remove a fighter}\n[N]{None}");
                                Console.ResetColor();

                            }
                            else if (tableChoice == 'N')
                            {

                            }
                            else
                            {
                                Console.ForegroundColor = ConsoleColor.Cyan;
                                Console.WriteLine("Invalid choice.\n>Enter A to Add\n>Enter D to remove\n>Enter N to skip");
                                Console.ResetColor();
                                //Console.Clear();
                            }                        
                        } while (tableChoice != 'N');
                        Console.Clear();
                        Console.ForegroundColor = ConsoleColor.Cyan;
                        Console.WriteLine($"<=====<=====<=====<=====||{selectedSport.GetType().Name} Associates||=====>=====>=====>=====>");
                        Console.ResetColor();
                        List<string> judges = GetJudges();
                        Console.Clear();
                        selectedSport.DisplayRulesAndRegulations();
                        WeightClassProcessor weightClassProcessor = new WeightClassProcessor();
                        List<Matches> matches = weightClassProcessor.ProcessWeightClass(fighters, weightclass);
                        List<Fighters> fightersMatch = GetPair(matches);
                        int numberOfRounds = GetNumberOfRounds(selectedSport);
                        Dictionary<Fighters, int> fighterScores = SimulateScoring(numberOfRounds, judges, fightersMatch);    
                        matchResultTxt = DisplayResults(fighterScores);
                    }
                    else if (loadChoice == 'N')
                    {
                        Console.ForegroundColor = ConsoleColor.Cyan;
                        Console.WriteLine($"<=====<=====<=====<=====||{selectedSport.GetType().Name} Associates||=====>=====>=====>=====>");
                        Console.ResetColor();
                        // Scoring System
                        List<string> judges = GetJudges();
                        int numberOfFighters = GetNumOfFighters();
                        fighters = GetFighters(numberOfFighters, selectedSport.GetName());
                        
                        WeightClassProcessor weightClassProcessor = new WeightClassProcessor();
                        
                        List<Matches> matches = weightClassProcessor.ProcessWeightClass(fighters, weightclass);
                        
                        List<Fighters> fightersMatch = GetPair(matches);
                       
                        int numberOfRounds = GetNumberOfRounds(selectedSport);
                        // Rules and Regulations
                        selectedSport.DisplayRulesAndRegulations();
                        // Simulated scores for demonstration
                        Dictionary<Fighters, int> fighterScores = SimulateScoring(numberOfRounds, judges, fightersMatch);
                        // Results                                      
                        matchResultTxt = DisplayResults(fighterScores);
                        
                    }
                    
                    string writeFile = "./Storedata.txt";
                    Filethis b = new Filethis();
                    string[] strings = new string[] { matchResultTxt };                  
                    fighters = b.WriteToFile("", writeFile, strings);
                    Console.ForegroundColor = ConsoleColor.Green;
                    Console.Write("Press any key to Continue......");
                    Console.ResetColor();
                    Console.ReadKey();
                    Console.Clear();
                } while (Y == 'Y' || Y == 'y');
            }
           catch (Exception ex)
            {
                Console.WriteLine($"An error occurred: {ex.Message}");
                Console.ReadKey();
            }
        }

        static List<Weightclass> InitiateWeightClass()
        {
            List<Weightclass> weightclass = new List<Weightclass>
            {
                new Weightclass("Flyweight", 126, 120),
                new Weightclass("Bantamweight", 136, 127),
                new Weightclass("Featherweight", 146, 137),
                new Weightclass("Lightweight", 156, 147),
                new Weightclass("Welterweight", 171, 157),
                new Weightclass("Middleweight", 186, 172),
                new Weightclass("LightHeavyweight", 206, 187),
                new Weightclass("Heavyweight", 265, 207)
            };
            return weightclass;
        }

        static CombatSport DisplayMainMenu()
        {
            Console.ForegroundColor = ConsoleColor.Green;
            Console.WriteLine("Select a Martial Art:");
            Console.ResetColor();
            Console.ForegroundColor = ConsoleColor.Green;
            Console.WriteLine("1. Kickboxing");
            Console.ResetColor();
            Console.ForegroundColor = ConsoleColor.Green;            
            Console.WriteLine("2. Boxing");
            Console.ResetColor();
            Console.ForegroundColor = ConsoleColor.Green;
            Console.WriteLine("3. MMA");
            Console.ResetColor();
            CombatSport selectedSport = null;
            int choice;
            do
            {
                Console.ForegroundColor = ConsoleColor.Green;
                Console.Write("Enter the number of your choice: ");
                Console.ResetColor();
                string choiceInput = Console.ReadLine();
                if (int.TryParse(choiceInput, out choice))
                {
                    switch (choice)
                    {
                        case 1:
                            selectedSport = new Kickboxing();
                            break;
                        case 2:
                            selectedSport = new Boxing();
                            break;
                        case 3:
                            selectedSport = new MMA();
                            break;
                        default:
                            Console.WriteLine("Invalid choice. Enter a number between 1 and 3.");
                            break;
                    }
                }
                else
                {
                    Console.WriteLine("Invalid input. Enter a number between 1 and 3.");
                }
            } while (choice < 1 || choice > 3);
            Console.Clear();
            return selectedSport;
        }


        static List<string> GetJudges()
        {
            List<string> judges = new List<string>();
            int judgename = 0;
            int i = 1;
            do
            {
                Console.ForegroundColor = ConsoleColor.Green;
                Console.Write($"\nEnter the name of Judge {i}: ");
                string input = Console.ReadLine();
                Console.ResetColor();
                if (int.TryParse(input, out judgename))
                {
                    Console.WriteLine($"Invalid name.");
                }
                else
                {
                    judges.Add(input);
                    i++;
                }
            } while (i <= 3);
            
            return judges;
        }

        
        static List<Fighters> GetFighters(int numberOfFighters, string eventSport)
        {
            List<Fighters> fighters = new List<Fighters>();
            int input = 1;
            Console.Clear();
            Console.ForegroundColor = ConsoleColor.Yellow;
            Console.WriteLine("<<<<<<<<<<<<<<<<Adding Participants>>>>>>>>>>>>>>>>");
            Console.ResetColor();
            Console.ForegroundColor = ConsoleColor.Cyan;
            Console.WriteLine("\n<WeightClass Guide>");
            Console.WriteLine(">Flyweight(120 - 126)");
            Console.WriteLine(">Bantamweight(127 - 136)");
            Console.WriteLine(">Featherweight(146, 137)");
            Console.WriteLine(">Lightweight(147 - 156)");
            Console.WriteLine(">Welterweight(157 - 171)");
            Console.WriteLine(">Middleweight(172 - 186)");
            Console.WriteLine(">LightHeavyweight(187 - 206)");
            Console.WriteLine(">Heavyweight(207 - 265)");
            Console.ResetColor();
            do
            {         
                string fighterName = "";
                int weight = 0;
                do
                {
                    Console.ForegroundColor = ConsoleColor.Green;
                    Console.Write($"\nEnter the name of fighter {input}: ");
                    string warrior = Console.ReadLine();
                    Console.ResetColor();
                    try
                    {                      
                        int.Parse(warrior);
                        Console.WriteLine($"Invalid Name");
                    }
                    catch (FormatException)
                    {
                        fighterName = warrior;
                    }
                } while (string.IsNullOrEmpty(fighterName) || fighterName.Trim() == "");
                do
                {
                    Console.ForegroundColor = ConsoleColor.Green;
                    Console.Write($"Enter the weight of the fighter {input}: ");
                    Console.ResetColor();
                    string numPound = Console.ReadLine();
                    try
                    {
                        weight = int.Parse(numPound);
                        Console.WriteLine();
                    }
                    catch (FormatException)
                    {
                        Console.WriteLine($"Invalid Weight");
                    }
                } while (weight == 0);
                //constructor
                fighters.Add(new Fighters(fighterName, weight, eventSport)); 
                input++;
            } while (input <= numberOfFighters);
            Console.Clear();
            return fighters;
        }

        static int GetNumOfFighters()
        {
            
            int num = 0;
            do
            {
                Console.ForegroundColor = ConsoleColor.Green;
                Console.Write($"\nNumber of fighters: ");
                Console.ResetColor();
                string numPound = Console.ReadLine();
                try
                {
                    num = int.Parse(numPound);
                    Console.WriteLine();
                }
                catch (FormatException)
                {
                    Console.WriteLine($"Invalid number");
                }
            } while (num == 0);
            Console.Clear();
            return num;
        }

        static int GetNumberOfRounds(CombatSport selectedSport)
        {
            int maxRounds;

            if (selectedSport is Boxing)
            {
                maxRounds = 12;
            }
            else
            {
                maxRounds = 5;
            }

            int numberOfRounds;
            do
            {
                Console.ForegroundColor = ConsoleColor.Green;
                Console.WriteLine("\nNumber of Rounds" + "");
                Console.Write($"Enter the number of rounds (maximum {maxRounds}): ");
                string input = Console.ReadLine();
                Console.ResetColor();
                if (int.TryParse(input, out numberOfRounds) && numberOfRounds >= 1 && numberOfRounds <= maxRounds)
                {
                    break;
                }
                else
                {
                    Console.WriteLine($"Invalid number of rounds. Please enter a value between 1 and {maxRounds}.");
                }
            } while (true);
            Console.Clear();
            return numberOfRounds;
        }

        static List<Fighters> GetPair(List<Matches> matches)
        {
            List<Fighters> fighters = new List<Fighters>();
            
            if (matches.Count == 1)
            {             
                Matches chosenMatch = matches[0];
                fighters.Add(chosenMatch.FighterOne);
                fighters.Add(chosenMatch.FighterTwo);
                return fighters;
            }
            Console.ForegroundColor = ConsoleColor.Green;
            Console.Write("\nEnter Pairing Number: ");
            int index = int.Parse(Console.ReadLine());
            Console.ResetColor();
            do
            {
                if (index > 0 && index < matches.Count)
                {
                    Matches chosenMatch = matches[index - 1];
                    fighters.Add(chosenMatch.FighterOne);
                    fighters.Add(chosenMatch.FighterTwo);
                    return fighters;
                }
                else
                {
                    Console.WriteLine("Invalid input. Please enter a valid pair number.");
                    Console.ForegroundColor = ConsoleColor.Green;
                    Console.Write("\nEnter Pairing Number: ");
                    index = int.Parse(Console.ReadLine());
                    Console.ResetColor();
                }
            } while(true);
        }

        static Dictionary<Fighters, int> SimulateScoring(int numberOfRounds, List<string> judges, List<Fighters> fighters)
        {
            try
            {
                Dictionary<Fighters, int> fighterScores = new Dictionary<Fighters, int>();
                Dictionary<Fighters, int> fighterPenalties = new Dictionary<Fighters, int>();

                foreach (var fighter in fighters)
                {
                    fighterScores[fighter] = 0;
                    fighterPenalties[fighter] = 0;
                }

                for (int round = 1; round <= numberOfRounds; round++)
                {
                    Console.ForegroundColor = ConsoleColor.Cyan;
                    Console.WriteLine($"\nRound {round}");
                    Console.ResetColor();
                    foreach (var judge in judges)
                    {                       
                        foreach (var fighter in fighters)
                        {
 
                            int score;
                            do
                            {
                                Console.ForegroundColor = ConsoleColor.Green;
                                Console.Write($"{judge}'s score for {fighter.Name}: ");
                                Console.ResetColor();
                                if (int.TryParse(Console.ReadLine(), out score))
                                {
                                    if (score >= 0 && score <= 10)
                                    {
                                        fighterScores[fighter] += score;
                                        break;
                                    }
                                    else
                                    {
                                        Console.WriteLine("Invalid score. Enter a score between 0 and 10.");
                                    }
                                }
                                else
                                {
                                    Console.WriteLine("Invalid input. Please enter a valid number.");
                                }
                            } while (true);
                        }
                    }
                    List<Fighters> disqualified = new List<Fighters>();
                    // Display penalties for each fighter after each round
                    foreach (Fighters fighter in fighters)
                    {
                        string penaltyInput;
                        do
                        {
                            Console.ForegroundColor = ConsoleColor.Green;
                            Console.Write($"Is there a penalty for {fighter.Name} in this round? (yes/no): ");
                            penaltyInput = Console.ReadLine().ToLower();
                            Console.ResetColor();
                        } while (penaltyInput != "yes" && penaltyInput != "no");

                        if (penaltyInput == "yes")
                        {
                            int numberOfPenalties;

                            do
                            {
                                Console.ForegroundColor = ConsoleColor.Green;
                                Console.Write($"Enter the number of penalties (1, 2, or 3) for {fighter.Name}: ");
                                Console.ResetColor();
                                if (int.TryParse(Console.ReadLine(), out numberOfPenalties) && numberOfPenalties >= 1 && numberOfPenalties <= 3)
                                {
                                    fighterPenalties[fighter] += numberOfPenalties;
                                    fighterScores[fighter] -= numberOfPenalties;

                                    if (fighterPenalties[fighter] >= 3)
                                    {
                                        Console.ForegroundColor = ConsoleColor.Cyan;
                                        Console.WriteLine($"{fighter.Name} is disqualified due to accumulating three penalties.");
                                        Console.ResetColor();
                                        //fighters.Remove(fighter)
                                        disqualified.Add(fighter);
                                        break;
                                    }

                                    break;
                                }
                                else
                                {
                                    Console.WriteLine("Invalid number of penalties. Enter 1, 2, or 3.");
                                }
                            } while (true);
                        }
                    }
                    foreach (var fighter in disqualified)
                    {
                        fighters.Remove(fighter);
                    }
                    if (fighters.Count == 1)
                    {
                        Console.ForegroundColor = ConsoleColor.Cyan;
                        Console.WriteLine($"{fighters[0].Name} is the winner!");
                        Console.ResetColor();
                        fighterScores.Remove(disqualified[0]);
                        break;
                    }
                    else if (fighters.Count == 0)
                    {
                        fighterScores = null;
                    }
                }
                Console.Clear();
                return fighterScores;
            }
            catch (Exception ex)
            {
                Console.WriteLine($"An error occurred in SimulateScoring: {ex.Message}");
                return null;
            }
        }

        static string DisplayResults(Dictionary<Fighters, int> fighterScores)
        {
            string result = "";
            
            try
            {
                Console.ForegroundColor = ConsoleColor.Yellow;
                Console.WriteLine("Fighter Scores:");
                Console.ResetColor();
                if (fighterScores == null)
                {
                    Console.ForegroundColor = ConsoleColor.Cyan;
                    result += "No Contest\n";
                    Console.ResetColor();
                    return result;
                }
                else if (fighterScores.Count == 1)
                {
                    Console.ForegroundColor = ConsoleColor.Cyan;
                    result += "\nWinner by Disqualification: " + fighterScores.Keys.First().Name;
                    Console.ResetColor();
                    return result;
                }
                
                foreach (var Scorefighter in fighterScores)
                {
                    Console.ForegroundColor = ConsoleColor.Cyan;
                    Console.WriteLine($"{Scorefighter.Key.Name}: {Scorefighter.Value}");
                    Console.ResetColor();
                }
                // Determine the winner and method of winning
                string winner = "";
                int maxScore = 0;
                int secondMaxScore = 0;
                bool isTie = false;
                foreach (var Scorefighter in fighterScores)
                {
                    if (Scorefighter.Value > maxScore)
                    {
                        secondMaxScore = maxScore;
                        maxScore = Scorefighter.Value;
                        Fighters a = Scorefighter.Key;
                        winner = a.Name;
                    }
                    else if (Scorefighter.Value == maxScore)
                    {
                        isTie = true;
                    }
                    else if (Scorefighter.Value > secondMaxScore)
                    {
                        secondMaxScore = Scorefighter.Value;                    
                    }                   
                }

                if (isTie)
                {
                    Console.ForegroundColor = ConsoleColor.Green;
                    result += "It's a draw!\n";
                    Console.ResetColor();
                }
                else if (maxScore - secondMaxScore == 1)
                {
                    Console.ForegroundColor = ConsoleColor.Green;
                    result += "Split Decision - Winner: " + winner;
                    Console.ResetColor();
                }
                else if ( maxScore - secondMaxScore >= 3) 
                {
                    Console.ForegroundColor = ConsoleColor.Green;
                    result += "\nUnanimous Decision - Winner: " + winner;
                    Console.ResetColor();
                }
                
            }
            catch (Exception ex)
            {
                Console.WriteLine($"An error occurred in DisplayResults: {ex.Message}");
            }
            return result;
        }

    }

}
