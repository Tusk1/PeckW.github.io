## Presented here are various small projects

**Project description:** Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.

### 1. Numerical Spinner
Developed in C# + Xamarin. A simple numerical spinner for number selection.

<img src="images/NS1.png"/>
<img src="images/NS2.png"/>

```C#
namespace Spinner_1
{
    /// <summary>
    /// Interaction logic for MainWindow.xaml
    /// </summary>
    public partial class MainWindow : Window
    {
        public double mValue;
               
        private void NumberValidationTextBox(object sender, TextCompositionEventArgs e)
        {
            Regex regex = new Regex("[^0-9.]+");
            e.Handled = regex.IsMatch(e.Text);
        }
        private void Button_Click_Up(object sender, RoutedEventArgs e)
        {
            mValue++;
            Data_Input.Text = mValue.ToString();
        }
        private void Button_Click_Down(object sender, RoutedEventArgs e)
        {
            mValue--;
            Data_Input.Text = mValue.ToString();
        }
        private void Data_Input_LostFocus(object sender, RoutedEventArgs e)
        {
            mValue = Double.Parse( Data_Input.Text );
        }
    }
}
```

### 2. Assess assumptions on which statistical inference will be based

```javascript
if (isAwesome){
  return true
}
```

### 3. Support the selection of appropriate statistical tools and techniques

<img src="images/dummy_thumbnail.jpg?raw=true"/>

### 4. Provide a basis for further data collection through surveys or experiments

Sed ut perspiciatis unde omnis iste natus error sit voluptatem accusantium doloremque laudantium, totam rem aperiam, eaque ipsa quae ab illo inventore veritatis et quasi architecto beatae vitae dicta sunt explicabo. 

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).
