UIAlertViewBlocks
=================

Category on UIAlertView to use inline block callbacks instead of delegate callbacks.

UIAlertView was created in a time before blocks, ARC, and judging by its naming – touch screens too. Who “clicks” on an alert view anyway?

Lets modernize this shizzle with some blocks goodness.

```objc
typedef void (^UIAlertViewBlock) (UIAlertView *alertView);
typedef void (^UIAlertViewCompletionBlock) (UIAlertView *alertView, NSInteger buttonIndex);

@property (copy, nonatomic) UIAlertViewCompletionBlock tapBlock;
@property (copy, nonatomic) UIAlertViewCompletionBlock willDismissBlock;
@property (copy, nonatomic) UIAlertViewCompletionBlock didDismissBlock;

@property (copy, nonatomic) UIAlertViewBlock willPresentBlock;
@property (copy, nonatomic) UIAlertViewBlock didPresentBlock;
@property (copy, nonatomic) UIAlertViewBlock cancelBlock;
```

You can create and show an alert in a single call, e.g.

```objc
[UIAlertView showWithTitle:@"Drink Selection"
                   message:@"Choose a refreshing beverage"
         cancelButtonTitle:@"Cancel"
         otherButtonTitles:@[@"Beer", @"Wine"]
                  tapBlock:^(UIAlertView *alertView, NSUInteger buttonIndex) {
                      if (buttonIndex == [alertView cancelButtonIndex]) {
                          NSLog(@"Cancelled");
                      } else if ([[alertView buttonTitleAtIndex:buttonIndex] isEqualToString:@"Beer"]) {
                          NSLog(@"Have a cold beer");
                      } else if ([[alertView buttonTitleAtIndex:buttonIndex] isEqualToString:@"Wine"]) {
                          NSLog(@"Have a glass of chardonnay");
                      }
                  }];
```

If you need further customization, you can create and configure an alert as you usually would, and then assign blocks to the alert, e.g.

```objc
UIAlertView *av = [[UIAlertView alloc] initWithTitle:@"Sign in to my awesome service"
                                             message:@"I promise I won’t steal your password"
                                            delegate:self
                                   cancelButtonTitle:@"Cancel"
                                   otherButtonTitles:@"OK", nil];

av.alertViewStyle = UIAlertViewStyleLoginAndPasswordInput;

av.tapBlock = ^(UIAlertView *alertView, NSInteger buttonIndex) {
    NSLog(@"Username: %@", [[alertView textFieldAtIndex:0] text]);
    NSLog(@"Password: %@", [[alertView textFieldAtIndex:1] text]);
};

av.cancelBlock = ^(UIAlertView *alertView) {
    NSLog(@"Cancelled.");
};

[av show];
```

If a delegate was set on the alert view, the delegate will be preserved and the blocks will be executed _before_ the delegate is called.

## Requirements

ARC - so iOS 4.3 or later

## Usage

Add `UIAlertView+Blocks.h/m` into your project, or `pod 'UIAlertViewBlocks'` using CocoaPods.

## Action Sheets

If you’d like similar functionality on UIActionSheet too, check out twin-sister [UIActionSheetBlocks](https://github.com/ryanmaxwell/UIActionSheetBlocks).