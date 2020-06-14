# Shopi Xamarin Architecture
Shopi Xamarin Example Project
## Models
- Use the same model with the API as the Base Model.
- Converting API models to Forms models is wasting time and effort.
- Create Extended Class for Forms. This will prevent posting models with unnecessary properties.
- Get casted Extended models from services and send casted Base models to them.
- If a property does change in runtime it is better to use Converters because they re-calls.(**try not to use for Lists; makes them slow**)
### *Example Model*
```
public class CustomerBase
{
  public int Id { get; set; }
  public string FirstName { get; set; }
  public string LastName { get; set; }
  public DateTime BirthDate { get; set; }
  public bool IsBlackbook { get; set; }
}

public class CustomerExtended : CustomerBase
{
  public string FullName { get => $"{FirstName} {LastName}"; }
  public string BirthDateUI { get => BirthDate.ToString("dd/MM/yyyy"); }
  public string Greeting { get => $"{AppResources.Hi} {FirstName}!"; }
  
  private bool _isExpanded;
  public bool IsExpanded
  {
    get => _isExpanded;
    set => SetProperty(ref _isExpanded, value);
  }
}
```

## Converters
- Use Converters for the modify properties those change in runtime
- Use Extended model if the property does not change in runtime
- Make Converters for name detailed.
- Use general and domain based converters
- Add general converters to App.xaml
- Do not call domain based converters in App.xaml, call them in View Resources
- Do not use for Heavy Listing

### *Example Converter*
```
public class DatetimeConverter : IValueConverter
{
  public object Convert(object value, Type targetType, object parameter, CultureInfo culture)
  {
    if(value is DateTime)
    {
      var date = (DateTime)value;
      return date.ToString("dd/MM/yyy HH:mm").ToUpper();
    }
    return "Value is Not Datetime";
  }
  
  public object ConvertBack(object value, Type targetType, object parameter, CultureInfo culture)
  {
    return value;
  }
}
```
App.Xaml file
```
<Application
  xmlns:converters="clr-namespace:DGCustomerApp.Converters">
  <Application.Resources>
    <ResourceDictionary>
      <converters:DatetimeConverter x:Key="DatetimeConverter" />
    </ResourceDictionary>
  </Application.Resources>
</Application>
```
