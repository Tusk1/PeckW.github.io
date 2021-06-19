## Type 1 Diabetes Management application

**Project description:** This project is a console application demonstrating functions and requirements that would be implemented into a fully developed application relating to management of Type 1 Diabetes for a better quality of life when living with the condition.

### 1. Overview

This is a .NET application purely using C# within Visual Studio 2017. The applications primary functions are: Conversion of units, addition and storage of blood glucose levels, account creation, login and insulin requirements calculator.

### 2. Application examples

<img src="images/DMConsoleHomePage.png"/>
<img src="images/DMConsoleCreateAccount.png"/>
<img src="images/DMConsoleMenu.png"/>
<img src="images/DMConsoleExample1.png"/>
<img src="images/DMConsoleExample2.png"/>

### 3. Code Examples
Unit Conversion
```C#
    class xDaMgToMmol
    {
        public static string MgValueAsString;
        public static int MgValueAsInt;
        public static int MgConvertedToMmolValue;

        public static void MgToMmol()
        {
            ClearAndDisplayMgToMmolConversion();

            Console.Write("Enter mg/dL value: ");

            MgValueAsString = string.Empty;
            MgValueAsString = Console.ReadLine();

            while (true)
            {
                if (!int.TryParse(MgValueAsString, out MgValueAsInt))
                {
                    Console.Write("This is an invalid value. Please press enter to try again.");
                    Console.ReadLine();

                    ClearAndDisplayMgToMmolConversion();

                    MgToMmol();
                }
                else
                {
                    break;
                }
            }

            MgConvertedToMmolValue = MgValueAsInt / xDaBloodGlucoseConverter.MmolToMgConversionRatio;

            Console.Write("Entered mg/dL = " + MgValueAsString + ". Converted to mmol/L value of " + MgConvertedToMmolValue + ".\n\n");

            Console.Write("\nWould you like to convert a different value?\nType: 'yes' or 'no'\n\n");
            xDaBloodGlucoseConverter.YesOrNoOption = Console.ReadLine();

            if (xDaBloodGlucoseConverter.YesOrNoOption == "yes" || xDaBloodGlucoseConverter.YesOrNoOption == "Yes")
            {
                ClearAndDisplayMgToMmolConversion();

                MgToMmol();
            }
            else if (xDaBloodGlucoseConverter.YesOrNoOption == "no" || xDaBloodGlucoseConverter.YesOrNoOption == "No")
            {
                Console.Clear();

                return;
            }
            else
            {
                Console.Write("\nInvalid option. Please type 'yes' or 'no'.\n\n");
                xDaBloodGlucoseConverter.YesOrNoOption = Console.ReadLine();
            }
        }

        public static void ClearAndDisplayMgToMmolConversion()
        {
            Console.Clear();

            Console.Write("-- mg/dL to mmol/L Conversion Calculator -- \n\n");
        }
    }
```

Insulin Requirement Calculation
```C#
    class xDaInsulinRequirementCalculator 
    {
        public static string TotalCarbohydrateValueForConversion;
        public static int InsulinValueForCalculation;
        public static int CarbValueForCalculation;
        public static int DefaultInsulinRatioValue = 1;
        public static int DefaultCarbRatioValue = 10;

        public static void InsulinRequirementsCalculator()
        {
            int StorageForInsulinInt;
            int StorageForCarbInt;
            int FinalInsulinRequirementValue;

            Program.ClearAndDisplayTitleOfApplication();

            ClearAndDisplayInsulinRequirementCalculator();

            Console.Write("Type 'New' for a new entry value. Type 'Load' to use exisiting values. ");
            string choice = Console.ReadLine();

            Console.Write("\n");

            if (choice == "New" || choice == "new")
            {
                Console.Write("\nThis ratio calculation works off two values:\n1. Insulin quantity(Units)\n2. Carbohydrate quantity(grams of carbohydrate).\nAn example would be  --> 1:10\n" + "Meaning you take 1 unit of insulin per 10grams of carbohydrate.\n\n");

                StorageForInsulinInt = InputOfInsulinRatioValue();

                StorageForCarbInt = InputOfCarbohydrateRatioValue();

                CalculationRequirements[] CalculationVariables =
                {
                    new CalculationRequirements(StorageForInsulinInt, StorageForCarbInt)
                };

                SaveToFile(CalculationVariables);

                Console.WriteLine(StorageForInsulinInt + "units per " + StorageForCarbInt + "grams.");

                InsulinValueForCalculation = StorageForInsulinInt;
                CarbValueForCalculation    = StorageForCarbInt;
            }
            else if (choice == "Load" || choice == "load")
            {
                CalculationRequirements[] CalculationVariables = LoadFromFile();

                if (InsulinValueForCalculation == 0)
                {
                    Console.Write("\nInsulin ratio value cannot be 0. 1 Unit has been set by default.\n");
                    InsulinValueForCalculation = DefaultInsulinRatioValue;
                }
                else
                {
                   
                }
                if (CarbValueForCalculation == 0)
                {
                    Console.Write("Carbohydrate ratio value cannot be 0. 10 grams has been set by default.\n\n");
                    CarbValueForCalculation = DefaultCarbRatioValue;
                }
                else
                {
                    
                }
                for(int i = 0; i < CalculationVariables.Length; ++i)
                {
                    Console.Write(CalculationVariables[i].InsulinQuantityValue + "unit/s per ");
                    Console.Write(CalculationVariables[i].CarbohydrateQuantityValue + "grams\n");
                }
            }
            else
            {
                Console.Write("Invalid Option, press 'Enter' to return.");
                Console.ReadLine();

                Console.Clear();

                return;
            }

            FinalInsulinRequirementValue = (InputOfTotalCarb() / CarbValueForCalculation) * InsulinValueForCalculation;

            Console.Write(FinalInsulinRequirementValue + " units of Insulin required.");
            Console.Write("\nPress 'Enter' key to return to menu.");
            Console.ReadLine();

            Console.Clear();
        }

        public static int InputOfInsulinRatioValue()
        {
            string InsulinQuantityValueAsString;

            Console.Write("Input an insluin ratio value (units): ");
            InsulinQuantityValueAsString = Console.ReadLine();
 
            if (Int32.TryParse(InsulinQuantityValueAsString, out int InsulinQuantityValueAsInt))
            {
                if(InsulinQuantityValueAsInt == 0)
                {
                    Console.WriteLine("\n0 is an invalid input. 1 unit has been set by default untill changed.\n");
                    return InsulinQuantityValueAsInt = 1;
                }
            }
            else
            {
                Console.Write("\nException: Could not parse InsulinQuantityValue\n");
            }
            return InsulinQuantityValueAsInt;
        }

        public static int InputOfCarbohydrateRatioValue()
        {
            string CarbohydrateQuantityValueAsString;

            Console.Write("Input an carbohydrate ratio value (grams): ");
            CarbohydrateQuantityValueAsString = Console.ReadLine();

            if (Int32.TryParse(CarbohydrateQuantityValueAsString, out int CarbohydrateQuantityValueAsInt))
            {
                if (CarbohydrateQuantityValueAsInt == 0)
                {
                    Console.WriteLine("\n0 is an invalid input. 10grams has been set by default untill changed.\n");
                    return CarbohydrateQuantityValueAsInt = 10;
                }
            }
            else
            {
                Console.Write("\nException: Could not parse CarbohydrateQuantityValue\n");
            }
            return CarbohydrateQuantityValueAsInt;
        }

        public static void SaveToFile(CalculationRequirements[] StoreVariables)
        {
            BinaryFormatter InsulinRequirementBinaryFormatter = new BinaryFormatter(); //to convert data to binary

            FileStream UserDataFileStream = File.Create(LogIn.UserName + ".bin"); //create and open binary file 

            InsulinRequirementBinaryFormatter.Serialize(UserDataFileStream, StoreVariables); // write data to file

            UserDataFileStream.Close(); //close file
        }

        public static CalculationRequirements[] LoadFromFile()
        {
            BinaryFormatter InsulinRequirementBinaryFormatter = new BinaryFormatter(); // create to convert from binary

            FileStream UserDataFileStream = File.OpenRead(LogIn.UserName + ".bin"); //open file containing data

            CalculationRequirements[] StoreVariables = (CalculationRequirements[])InsulinRequirementBinaryFormatter.Deserialize(UserDataFileStream); //read and convert data

            UserDataFileStream.Close(); // close file

            return StoreVariables;
        }

        public static int InputOfTotalCarb()
        {
            Console.Write("\nInput total carbohydrate value for conversion (grams): ");
            TotalCarbohydrateValueForConversion = Console.ReadLine();

            if (Int32.TryParse(TotalCarbohydrateValueForConversion, out int TotalCarbohydrateValueForConversionAsInt))
            {
                if (TotalCarbohydrateValueForConversionAsInt == 0)
                {
                    Console.WriteLine("\n0 is an invalid input. 50grams has been set by default untill changed.\n");
                    return TotalCarbohydrateValueForConversionAsInt = 50;
                }
            }
            else
            {
                Console.Write("\nException: Could not parse TotalCarbohydrateValueForConversion\n");
            }
            return TotalCarbohydrateValueForConversionAsInt;
        }
```
