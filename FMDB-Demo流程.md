# List页面逻辑

```
viewDidLoad 
	createTable
	loadStuInfo

loadStuInfo	
	if (local) {
		loadStuInfoLocal
		return;
	} 
	网络请求
	更新界面 reloadData
	保存数据库 updateDB			
	
loadStuInfoLocal
{
	_students = [NSMutableArray array];
    
    NSString *sql = @"select * from Students";

    // 打开数据库
    if (![_database open]) {
        NSLog(@"Open database failed!");
        return;
    }
    
    // 执行查询操作，并取出结果保存到数组_students中
    FMResultSet *set = [_database executeQuery:sql];
    while ([set next]) {

        NSString *name = [set stringForColumn:kNameKey];
        NSString *stuID = [set stringForColumn:kIDKey];
        int age = [set intForColumn:kAgeKey];

        YMStudentModel *student = [[YMStudentModel alloc] initWithName:name age:age stuID:stuID];

        [_students addObject:student];
    }
    
    [self.tableView reloadData];
}	

updateDB
{
    // 打开数据库
    if (![self.database open]) {
        NSLog(@"Open database failed!");
        return;
    }
    
    // 执行更新
    for (NSDictionary *stuDict in _students) {
        NSString *sql = [NSString stringWithFormat:@"insert into Students(stu_id, name, age) values (:%@, :%@, :%@)", kIDKey, kNameKey, kAgeKey];
        [_database executeUpdate:sql withParameterDictionary:stuDict];
    }
    
    // 关闭数据库
    [_database close];
    
    [[NSUserDefaults standardUserDefaults] setBool:YES forKey:kTableNotEmpty];
}

```

# Detail页面逻辑

```
- (IBAction)saveData:(id)sender {
    if (!self.contentChanged) {
        NSLog(@"No change!");
        return;
    }
    
    [self updateStudentInfo];
}


- (void)updateStudentInfo
{
    NSString *sql = @"update Students set name = ?, age = ? where stu_id = ?";
    
    // 打开数据库
    if (![self.database open]) {
        NSLog(@"Open database failed!");
        return;
    }
    
    // 执行更新操作
    [_database executeUpdate:sql, _name.text, _age.text, _number.text];
    
    // 关闭数据库
    [_database close];
    
    // 更新学生记录信息
    _studentInfo.name = _name.text;
    _studentInfo.age = [_age.text intValue];
    _studentInfo.stu_id = _number.text;
    
    [[NSNotificationCenter defaultCenter] postNotificationName:kStuInfoModified object:nil];
    
    NSLog(@"Update OK!");
}
	
```