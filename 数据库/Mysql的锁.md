1. mysql锁分类，有哪些
    
    - 悲观锁:
        - 共享锁【shared locks】又称为读锁，简称 S 锁
        - 排他锁【exclusive locks】又称为写锁，简称 X 锁
    - 乐观锁
        - cas
        - 版本号
    - 行锁
    - 表锁
    - 间隙锁
2. 锁的应用 （事务的隔离级别）
3. 不同的隔离级别，不同的索引情况下分别加什么锁