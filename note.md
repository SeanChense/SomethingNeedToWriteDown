1. 设置点击键盘
	1）实现 `delegate` 方法中的`textFieldShouldReturn`方法，并且找个地方把输入框设置代理：`TextField.delegate = self;`并且为了改变键盘的回车样式：`self.TextField.returnKeyType = UIReturnKeyNext;`
2. 设置输入框监听事件	
	1）`Editing Changed`是可以时时追踪
	2）`Editing Did End` 是监听离开焦点
3. 设置 `indicator`
	1）在代码
		 `[self.view addSubview:spinner];`
	   代码中并看不到内容，反而是把代码变成
	    	 `[self.view.subviews[0] addSubview:spinner];`
	   才能从界面中看到效果,或者要
		 `[self.view.windows addSubview:spinner];`
	   能看到效果
4. 在 `prepareForSegue` 中给下一个 `ViewController` 中的属性赋值时使用
		 `vc.Label.text = @"dd";` 是不行的，只能
		 `vc.String = @"dd";`
		 `self.Label.text = self.String;`才可以。
5. 把 `Segue` 连在 `viewcontroller` 上，使用
		`[self performSegueWithIdentifier:@"My Segue"];`
  可以触发指定 `Segue`
6. `Segue` 不能连接到 `navigation` 上面去，会报错。
7. `git remote -v `可以查看仓库信息。
8. 在 `NSDictionary `的拼装过程中，如果中途有某个参数为 nil 的话拼装会提前终止。
9. 在 `navigation bar` 中设置一个搜索框
	`[self.navigationItem setTitleView:searchbar];`
10. 由于 `UISearchBar` 在 `CODE` 中生成出来时是没有办法直接设置 `cliearbutton` 的，只能曲线救国。

    	//set clear button for UISeachBar
        for (UIView* v in searchbar.subviews)
        {
            if ([v isKindOfClass:[UITextField class]]) {
                UITextField* tf = (UITextField*)v;
                tf.delegate = self;
                tf.clearButtonMode = UITextFieldViewModeAlways;
                break;
            }
        }

11. 从 `navigationbar` 到一个新的 `viewcontroller` 的连接不是普通的 segue 而是 rootview，这样才不会跳出一个黑屏幕。
12. 十分重要！！！！！
   在给 `tableviewcell` 指定相应的指定 `cell` 子类时，也要把 `storyboard` 上的控件和 `cell.h` 文件中的 `property` 相连接。
13. 给 UIBarButtonItem 设置 font

      	UIBarButtonItem* btn_search = [[UIBarButtonItem alloc] initWithTitle:[NSString fontAwesomeIconStringForIconIdentifier:@"fa-search"] style:UIBarButtonItemStyleBordered target:self action:@selector(btn_searchTapped)];
        //set UIBarButtonItem font
      	[btn_search setTitleTextAttributes:[NSDictionary dictionaryWithObjectsAndKeys:[UIFont fontWithName:kFontAwesomeFamilyName size:28],NSFontAttributeName, nil] forState:UIControlStateNormal];
        self.navigationItem.rightBarButtonItem = btn_search;

14. 在 `navigationbar` 上的两边设置多个 UIBarButtonItem 可以
	`self.navigationItem.rightBarButtonItems = @[btn_1,btn_2];`
注意这里有个 s 。
15. NSJSONSerialization returns immutable objects by default.So you need get the result like this:
	`[resultData mutableCopy];`
16. 在做删除列表 `cell` 的过程中，要注意先删除数据源再动画。不然会有很多错。
17. 在使用 `MBProgress` 的时候遇到 `uitableview` 会出现 `cell lines` 划过黑色背景的情况。解决办法就是在展示 MBP 的时候把分割线隐藏起来
	 `[self.tableView setSeparatorStyle:UITableViewCellSeparatorStyleNone];`
等到 MBP 展示完成就把分割线还原回来。
	`[self.tableView setSeparatorStyle:UITableViewCellSeparatorStyleSingleLine];`
18. 在使用动画操作

        [UIView animateWithDuration:1.0
                        delay:0.0
                      options:UIViewAnimationOptionCurveEaseInOut
                   animations:^{
                       [self.viewHolder setFrame:rectForAnimationAfter];
                   }
                   completion:nil];

的时候 相应的布局界面是设置的 `utolayout` 这样是不被允许操作 `frame` 的。所以可能会看不到效果或者效果出来了，马上就还原回去了。所以把 autolayout 取消掉。
19. 动态的 `tableview` 中的 `cell` 的高度不是能通过 `storyboard `直接指定的，可能这是个苹果的 bug 在 `heightForRowAtIndexPath` 函数里执行	

        static NSString* cellForIdentifier;
      	if (indexPath.section == 0)
     	 {
          if (indexPath.row ==0)
              cellForIdentifier = @"firstCell";
          else
              cellForIdentifier = @"secondCell";
      	}
      	else
      	{
       		cellForIdentifier = @"secondCell";
      	}
      	UITableViewCell *cell = [tableView dequeueReusableCellWithIdentifier:cellForIdentifier];
      	return cell.bounds.size.height;


do the trick to adjust cell height

20.每一个 section 里的 row 都是从 0 开始重新计数的，可以看作是二维数组。

21.用代码设置 button 的文字时，会发现 btn.titleLabel 中的文字没办法填满 button

    	`btn.titleLabel.adjustFontSizeToFitWidth = YES;`

22.代码生成的 tableView 在 init 的时候选择了 style 为 grouped cell 与 tableview 之间就会有留空。

23.做到跟微信修改昵称一样的界面弹跳出来+navigation 的效果，需要


        ChangeNickNameViewController* cnnVC = [story instantiateViewControllerWithIdentifier:@"ChangeNickName"];
        UINavigationController* mNavigation = [[UINavigationController alloc] initWithRootViewController:cnnVC];
        cnnVC.oldNickName = [WeshareSingleton weshareSingleton].nickname;
        [self.navigationController presentViewController:mNavigation animated:YES completion:nil];


其中 cnnVC.oldNickName 赋值的语句是用 present 方式页面跳转传递值
24. 在使用了 presentView 之后使用 dismissview 可以返回23.做到跟微信修改昵称一样的界面弹跳出来+navigation 的效果，需要

         ChangeNickNameViewController* cnnVC = [story instantiateViewControllerWithIdentifier:@"ChangeNickName"];
        UINavigationController* mNavigation = [[UINavigationController alloc] initWithRootViewController:cnnVC];
        cnnVC.oldNickName = [WeshareSingleton weshareSingleton].nickname;
        [self.navigationController presentViewController:mNavigation animated:YES completion:nil];

其中 `cnnVC.oldNickName` 赋值的语句是用 present 方式页面跳转传递值
25. 当在一个 viewcontroller 中使用多个 alert 的时候，函数
	`-(void)alertView:(UIAlertView *)alertView clickedButtonAtIndex:(NSInteger)buttonIndex`
会无法识别动作是来自哪个 alert 发出来的。alert.tag = Integer 是个好东西。
可以在函数里做一个 switch 判断。
26. 判断某个字符串 A 是否包含字符串 B 
	`[A rangeOfString:B].location !=NSNotFound`
27. 得到当前 UIWebView 所展现页面的 URL 实现 UIWebViewDelegate 委托方法

    	-(void)webViewDidFinishLoad:(UIWebView *)webView
    	{
      		 currentUrlStr = _webView.request.mainDocumentURL.absoluteString;
        
    	}



> 过了这么久，iOS 终于打算重新开始做了 =、=

28. xib 生成一个 cell 重用控件，需要在代码中手动更新 cell 的高度，毕竟是动态的。
	
