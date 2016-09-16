---
title: iOS--使用UISearchController创建搜索框
date: 2016-08-29 16:40:27  
categories: 
- iOS开发总结
tags: 
- iOS
---
&emsp;&emsp;在iOS8之前，我们一直利用UISearchBar和UISearchDisplayController来实现搜索框，但是在iOS8以后上述方法已经过时，改而实用UISearchController来替代，用起来比之前的方法要简单好多。  
## 添加UISearchController变量 
&emsp;&emsp;在开始之前我们要添加UISearchController变量，并未搜索前的数据和搜索结果创建两个数组：  

<pre>@property(strong, nonatomic) UISearchController *searchController;
@property(strong, nonatomic) NSMutableArray *bookArray;
@property(strong, nonatomic) NSMutableArray *bookResult;</pre>

## 初始化UISearchController
&emsp;&emsp;初始化UISearchController并设置其属性和默认文字  

<pre>- (void)showSearchBar {
    _searchController = [[UISearchController alloc] initWithSearchResultsController:nil];
    //搜索结果处理函数代理
    _searchController.searchResultsUpdater = self;
    //是否显示背景
    _searchController.dimsBackgroundDuringPresentation = false;
    [_searchController.searchBar sizeToFit];
    //修改searchBar的默认文字
    _searchController.searchBar.placeholder = @"搜索";
    //修改“Cancle按钮的默认文字”
    [_searchController.searchBar setValue:@"取消" forKey:@"_cancelButtonText"];
    //将搜索框添加到tableHeaderView中
    _CheckBookViewtableView.tableHeaderView = self.searchController.searchBar;
}</pre>

## 添加UISearchResultsUpdating协议  
&emsp;&emsp;如果要对搜索结果进行处理，那我们要为UISearchController添加一个搜索结果处理的协议，并实现其下的方法：  
&emsp;&emsp;添加协议：

<pre>@interface CheckBookViewController ()< UITableViewDataSource,UITableViewDelegate,UISearchResultsUpdating ></pre>

&emsp;&emsp;实现方法：

<pre>- (void)updateSearchResultsForSearchController:(UISearchController *)searchController {
    [searchResult removeAllObjects];
    NSString *keyWord = [searchController .searchBar text];
    [bookArray enumerateObjectsUsingBlock:^(id  _Nonnull obj, NSUInteger idx, BOOL * _Nonnull stop) {
        Book *book = obj;
        if ([book.bookName.uppercaseString containsString:keyWord.uppercaseString]) {
            [searchResult addObject:book];
        }
    }];
    [_CheckBookViewtableView reloadData];
}</pre>

## UITableView代理方法下对搜索结果的处理
&emsp;&emsp;因为搜索结果要复用界面的TableView，所以在TableView的代理方法下要进行判断当前是搜索的界面还是主界面：

<pre>- (NSInteger)tableView:(UITableView *)tableView numberOfRowsInSection:(NSInteger)section {
    // 判断是否是搜索结果的tableView
    if ([self.searchController isActive]) {
        return searchResult.count;
    }
    return [bookArray count];
}
- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath {
    Book *books;
    CheckBookViewcell = [tableView dequeueReusableCellWithIdentifier:@"UIListTableViewCell"];                                                        // 从缓存池中取出cell
    if (!CheckBookViewcell) {                                                                                                                        // 判断是否能取出cell
        CheckBookViewcell = [[ListTableViewCell alloc]initWithStyle:UITableViewCellStyleSubtitle reuseIdentifier:@"UIListTableViewCell"];            // 如果cell为空则创建一个新的cell并放入缓存池中
    }else{                                                                                                                                           // 如果cell不为空（注意：以下操作很重要，不然会造成cell数据错乱）
        [CheckBookViewcell removeCellView];                                                                                                          // 将之前cell界面上的view全部remove掉
        [CheckBookViewcell initCellView];                                                                                                            // 重新初始化cell上的view
    }
    // 判断是否是搜索结果的tableView
    if ([self.searchController isActive]) {
        books = searchResult[indexPath.row];
    }else {
        if (viewcode) {
            books = bookArray[indexPath.row];
        }else {
            books = bookArray[indexPath.row];
        }
    }
    
    [CheckBookViewcell setBookInfo:books];
    return CheckBookViewcell;
}
- (void)tableView:(UITableView *)tableView didSelectRowAtIndexPath:(NSIndexPath *)indexPath {
    Book *book;
    if ([_searchController isActive]) {
        book = searchResult[indexPath.row];
    }else {
        book = bookArray[indexPath.row];
    }
    BookDetialViewController *bookDetialVC = [[BookDetialViewController alloc]init:book];
    [self.navigationController pushViewController:bookDetialVC animated:YES];
    [tableView deselectRowAtIndexPath:indexPath animated:YES];                                                                                  // 取消选中的状态
}</pre>

## 最后注意!
&emsp;&emsp;在离开界面时要销毁掉UISearchController，不然它会在其它界面一直存在：
<pre>- (void)viewWillDisappear:(BOOL)animated {
    [super viewWillDisappear:animated];
    if (self.searchController.active) {
        self.searchController.active = NO;
        [self.searchController.searchBar removeFromSuperview];
    }
}</pre>
