# Client-side Implementation

You will need the following frameworks to implement the client-side of BirdTube.

* YouTube iOS Player Helper
 
 [https://github.com/youtube/youtube-ios-player-helper](https://github.com/youtube/youtube-ios-player-helper)

 Framework needed to playback YouTube videos on your iOS app

* SendBird
 
 [https://github.com/smilefam/sendbird-ios-framework](https://github.com/smilefam/sendbird-ios-framework)

 Framework needed to implement public chat for the videos

* hpple

 [https://github.com/topfunky/hpple](https://github.com/topfunky/hpple)

 Framework used to parse HTML to retrieve YouTube video’s video ID, title, and thumbnail URL

* AFNetworking

 [https://github.com/AFNetworking/AFNetworking](https://github.com/AFNetworking/AFNetworking)
 
 Framework used to load images on UIImageView from a URL
 
## User Signup Process

A signup screen will be displayed when a non-member user tries to submit a video or when a user taps ‘Sign Up’ from the ‘Settings’ screen. Email and password are required for the signup.

```objectivec
// SignUpViewController.m

- (IBAction)signUp:(id)sender {
    NSString *email = [self.emailTextField text];
    NSString *password = [self.passwordTextField text];
    if ([email length] == 0 || [password length] == 0) {
        return;
    }
    NSString *nickname = [[email componentsSeparatedByCharactersInSet:[NSCharacterSet characterSetWithCharactersInString:@"@"]] objectAtIndex:0];
    if ([nickname length] == 0) {
        return;
    }
    NSString *deviceToken = @"";
    [self.indicatorView setHidden:NO];
    [self.indicatorView startAnimating];
    [Server signUpWithEmail:email nickname:nickname password:password deviceToken:deviceToken resultBlock:^(NSDictionary *response, NSError *error) {
        if (error) {
            UIAlertController *alert = [UIAlertController
                                        alertControllerWithTitle:@"Error"
                                        message:[error domain]
                                        preferredStyle:UIAlertControllerStyleAlert];
            
            UIAlertAction* closeButton = [UIAlertAction
                                          actionWithTitle:@"Close"
                                          style:UIAlertActionStyleDefault
                                          handler:^(UIAlertAction * action) {
                                              
                                          }];
            
            [alert addAction:closeButton];
            
            [self presentViewController:alert animated:YES completion:nil];
        }
        else {
            if ([[response objectForKey:@"result"] isEqualToString:@"error"]) {
                UIAlertController *alert = [UIAlertController
                                            alertControllerWithTitle:@"Error"
                                            message:[response objectForKey:@"message"]
                                            preferredStyle:UIAlertControllerStyleAlert];
                
                UIAlertAction* closeButton = [UIAlertAction
                    actionWithTitle:@"Close"
                    style:UIAlertActionStyleDefault
                    handler:^(UIAlertAction * action) {
                        
                    }];
                
                [alert addAction:closeButton];
                
                [self presentViewController:alert animated:YES completion:nil];
            }
            else {
                NSDictionary *userDict = [response objectForKey:@"user"];
                NSString *email = [userDict objectForKey:@"email"];
                NSString *nickname = [userDict objectForKey:@"nickname"];
                NSString *session = [userDict objectForKey:@"session"];
                NSString *sendbird_id = [userDict objectForKey:@"sendbird_id"];
                [MyUtils setUserID:email];
                [MyUtils setUserName:nickname];
                [MyUtils setSession:session];
                [MyUtils setSendBirdID:sendbird_id];
                UIAlertController *alert = [UIAlertController
                                         alertControllerWithTitle:@"Welcome"
                                         message:@"Thank you for signing up."
                                         preferredStyle:UIAlertControllerStyleAlert];
                
                UIAlertAction* closeButton = [UIAlertAction
                                              actionWithTitle:@"Close"
                                              style:UIAlertActionStyleDefault
                                              handler:^(UIAlertAction * action) {
                                                  [self dismissViewControllerAnimated:YES completion:nil];
                                                  [self.delegate refreshSignUpStatus];
                                              }];
                
                [alert addAction:closeButton];
                
                [self presentViewController:alert animated:YES completion:nil];
            }
        }
        [self.indicatorView setHidden:YES];
        [self.indicatorView stopAnimating];
    }];
}
```

## User Login Process

A login screen will be displayed when a non-member user tries to submit a video or when a user taps ‘Sign In’ from the ‘Settings’ screen. Email and password are required for the login.

```objectivec
// SignInViewController.m

- (IBAction)signIn:(id)sender {
    NSString *email = [self.emailTextField text];
    NSString *password = [self.passwordTextField text];
    if ([email length] == 0 || [password length] == 0) {
        return;
    }
    NSString *deviceToken = @"";
    [Server loginWithEmail:email password:password deviceToken:deviceToken resultBlock:^(NSDictionary *response, NSError *error) {
        if (error) {
            UIAlertController *alert = [UIAlertController
                                        alertControllerWithTitle:@"Error"
                                        message:[error domain]
                                        preferredStyle:UIAlertControllerStyleAlert];
            
            UIAlertAction* closeButton = [UIAlertAction
                                          actionWithTitle:@"Close"
                                          style:UIAlertActionStyleDefault
                                          handler:^(UIAlertAction * action) {
                                              
                                          }];
            
            [alert addAction:closeButton];
            
            [self presentViewController:alert animated:YES completion:nil];
        }
        else {
            if ([[response objectForKey:@"result"] isEqualToString:@"error"]) {
                UIAlertController *alert = [UIAlertController
                                            alertControllerWithTitle:@"Error"
                                            message:[response objectForKey:@"message"]
                                            preferredStyle:UIAlertControllerStyleAlert];
                
                UIAlertAction* closeButton = [UIAlertAction
                                              actionWithTitle:@"Close"
                                              style:UIAlertActionStyleDefault
                                              handler:^(UIAlertAction * action) {
                                                  
                                              }];
                
                [alert addAction:closeButton];
                
                [self presentViewController:alert animated:YES completion:nil];
            }
            else {
                NSDictionary *userDict = [response objectForKey:@"user"];
                NSString *email = [userDict objectForKey:@"email"];
                NSString *nickname = [userDict objectForKey:@"nickname"];
                NSString *session = [userDict objectForKey:@"session"];
                NSString *sendbird_id = [userDict objectForKey:@"sendbird_id"];
                [MyUtils setUserID:email];
                [MyUtils setUserName:nickname];
                [MyUtils setSession:session];
                [MyUtils setSendBirdID:sendbird_id];
                UIAlertController *alert = [UIAlertController
                                            alertControllerWithTitle:@"Welcome back"
                                            message:@"Thank you for signing in."
                                            preferredStyle:UIAlertControllerStyleAlert];
                
                UIAlertAction* closeButton = [UIAlertAction
                                              actionWithTitle:@"Close"
                                              style:UIAlertActionStyleDefault
                                              handler:^(UIAlertAction * action) {
                                                  [self dismissViewControllerAnimated:YES completion:^{
                                                      [self.delegate refreshSignInStatus];
                                                  }];
                                              }];
                
                [alert addAction:closeButton];
                
                [self presentViewController:alert animated:YES completion:nil];
            }
        }
    }];
}
```

## Submitting YouTube Video

We need to retrieve video ID, title, and thumbnail URL from the parsed HTML that’s submitted through the submitted URL. We’ll be using hpple Framework for the job.

```objectivec
// Server.m

+ (void) getYouTubeInfoUrl:(NSString *)url resultBlock:(void (^)(NSDictionary *response, NSError *error))onResult
{
    NSMutableURLRequest *request = [[NSMutableURLRequest alloc] init];
    [request setHTTPMethod:@"GET"];
    [request setValue:@"SendBird Messenger/0.9.0" forHTTPHeaderField:@"User-Agent"];
    [request setURL:[NSURL URLWithString:url]];
    
    [NSURLConnection sendAsynchronousRequest:request queue:[NSOperationQueue mainQueue] completionHandler:^(NSURLResponse *response, NSData *data, NSError *connectionError) {
        NSString *videoId = @"";
        NSString *thumbnailUrl = @"";
        NSString *name = @"";
        NSMutableDictionary *result = [[NSMutableDictionary alloc] init];
        
        NSError *error = nil;
        if (connectionError) {
            error = connectionError;
            [result setObject:@"error" forKey:@"result"];
            [result setObject:@"message" forKey:@"Connection error."];
        }
        else {
            TFHpple *doc = [[TFHpple alloc] initWithHTMLData:data];
            TFHppleElement *videoIdelements = [doc peekAtSearchWithXPathQuery:@"//meta[@itemprop='videoId']"];
            TFHppleElement *thumbnailUrlElements = [doc peekAtSearchWithXPathQuery:@"//link[@itemprop='thumbnailUrl']"];
            TFHppleElement *nameIdelements = [doc peekAtSearchWithXPathQuery:@"//meta[@itemprop='name']"];
            videoId = [videoIdelements objectForKey:@"content"];
            thumbnailUrl = [thumbnailUrlElements objectForKey:@"href"];
            name = [nameIdelements objectForKey:@"content"];
            
            [result setObject:@"success" forKey:@"result"];
            [result setObject:videoId forKey:@"videoId"];
            [result setObject:thumbnailUrl forKey:@"thumbnailUrl"];
            [result setObject:name forKey:@"name"];
        }

        NSBlockOperation *op = [[NSBlockOperation alloc] init];
        [op addExecutionBlock:^{
            onResult(result, error);
        }];
        [[NSOperationQueue mainQueue] addOperation:op];
    }];
}
```

![](../file/009_screenshot.png =320x)
![](../file/010_screenshot.png =320x)