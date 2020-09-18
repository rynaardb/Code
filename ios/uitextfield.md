# UITextField

## Create a custom clear button

```objectivec
UIButton *button = [UIButton buttonWithType:UIButtonTypeCustom];
[button setImage:[UIImage imageNamed:@"clear_button.png"] forState:UIControlStateNormal];
[button setFrame:CGRectMake(0, 0, 20, 20)];
theTextField.rightView = button;
theTextField.rightViewMode = UITextFieldViewModeWhileEditing;
```