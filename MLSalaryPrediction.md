## Machine Learning - Salary Predictor 

**Project description:** The project is designed to predict a salary from quantity of experience in years.

### 1. Overview

The code is hand written to help practise the development and design of building a machine learning AI. Built in visual studio modeled off the microsoft machine learning kit.

### 2. Result

<img src="images/dummy_thumbnail.jpg?raw=true"/>

### 3. Code Examples

```C#
namespace MLNETHandWritten
{
    class Program
    {
        static void Main(string[] args)
        {
            var context = new MLContext();

            //Follow the build path of:

            //LOAD DATA
            var trainData = context.Data.LoadFromTextFile<SalaryData>("./MOCK_DATA.csv", hasHeader: true, separatorChar: ',');
            //defualt seperatorChar is 'Tab'

            var testTrainSplit = context.Data.TrainTestSplit(trainData, testFraction: 0.2);
            //splits the data into 80% learning data and 20% test data which we can use to test the model. Gives a 'TrainSet' and a 'TestSet'.

            //BUILD THE MODEL
            //inside the Concatenate brackets contains first the output to be followed by the input which can be as many columns as desired
            var pipeline = context.Transforms.Concatenate("Features", "YearsExperience").Append(context.Regression.Trainers.LbfgsPoissonRegression());

            var model = pipeline.Fit(testTrainSplit.TrainSet);

            //EVALUATE THE DATA
            var predictions = model.Transform(testTrainSplit.TestSet);

            var metrics = context.Regression.Evaluate(predictions);
            //dates the predictions data and evaluates it

            Console.WriteLine($"R^2 = {metrics.RSquared}");
            //R^2 states that the closer to 1 it is the better off the model performs

            //PREDICT
            var newData = new SalaryData
            {
                YearsExperience = 4.5f
                //model uses this data for new prediction
            };

            var predictionFunction = context.Model.CreatePredictionEngine<SalaryData, SalaryPrediction>(model);

            var prediction = predictionFunction.Predict(newData);

            Console.WriteLine($"Prediction = {prediction.PredictedSalary}");

            Console.ReadLine();
        }
    }
}
```

