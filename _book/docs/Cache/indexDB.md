```
const MYDB = {
  name: 'test',
  version: 1,
  objstore: {
    name: 'student', // 存储空间表名
    keypath: 'id' // 主键
  }
};

const students = [{ 
  id: 1001, 
  name: "Byron", 
  age: 24 
  }, { 
  id: 1002, 
  name: "Frank", 
  age: 30 
  }, { 
  id: 1003, 
  name: "Aaron", 
  age: 26 
}];

const INDEXDB = {
  indexedDB: window.indexedDB || window.mozIndexedDB || window.webkitIndexedDB || window.msIndexedDB, // 各浏览器兼容
  IDBKeyRange:window.IDBKeyRange || window.webkitIDBKeyRange, // 键范围

  openDB: function(dbName, dbVersion = 1) { // use DB
    const self = this;

    return new Promise(resolve => { 
      const request = self.indexedDB.open(dbName, dbVersion);

      request.onerror = function(e) {
        console.error(e.currentTarget.error.message);
      };
  
      request.onsuccess = function (e) {
        return resolve(e.target.result);
      };
  
      request.onupgradeneeded = function(e) {
        const db = e.target.result;
        const transaction = e.target.transaction;
        let store;
  
        if (!db.objectStoreNames.contains(MYDB.objstore.name)) { // 没有该对象空间时创建该对象空间 
          store = db.createObjectStore(MYDB.objstore.name, { keyPath: MYDB.objstore.keypath });
          console.log('成功建立对象存储空间：', MYDB.objstore.name);
        }
      };
    })
  },

  closeDB: function(db) {
    db.close();
  },

  deleteDB: function (db) { // 删除数据库
    const self = this;
    self.indexedDB.deleteDatabase(db);
  },

  addData: function(db, storeName, data) { // 添加数据，重复添加会报错
    const store = db.transaction(storeName, 'readwrite').objectStore(storeName);
    let request;
    for(let i = 0 ; i < data.length; i++) {
      request = store.add(data[i]);
      request.onerror = function() {
        console.error('数据库中已有该数据');
      };
      request.onsuccess = function() {
        console.log('数据存入数据库');
      };
    }
  },

  putData: function(db, storeName, data) { // 添加原数据，重复添加更新原有数据
    const store = db.transaction(storeName,'readwrite').objectStore(storeName);
    let request;
    for(let i = 0 ; i < data.length;i++) {
      request = store.put(data[i]);
      request.onerror = function() {
        console.error('put添加数据库中已有该数据');
      };
      request.onsuccess = function() {
        console.log('put添加数据已存入数据库');
      };
    }
  },

  getDataByKey: function(db, storeName, key) { // 根据存储空间的键找到对应数据 
    const store = db.transaction(storeName, 'readwrite').objectStore(storeName);
    const request = store.get(key);
    request.onerror = function() {
      console.error('getDataByKey error');
    };
    request.onsuccess = function(e) {
      const result = e.target.result;
      console.log('查找数据成功');
      console.log(result);
    };
  },

  deleteData: function(db, storeName, key) { // 删除某一条记录
    const store = db.transaction(storeName, 'readwrite').objectStore(storeName);
    store.delete(key)
    console.log(`已删除存储空间${storeName}中${key}记录`);
  },

  clearData: function(db, storeName) { // 删除存储空间全部记录
    const store = db.transaction(storeName, 'readwrite').objectStore(storeName);
    store.clear();
    console.log(`已删除存储空间${storeName}全部记录`);
  }
};

async function run() {
  MYDB.db = await INDEXDB.openDB(MYDB.name, MYDB.version);

  console.log('******************添加数据*****************************');
  INDEXDB.addData(MYDB.db , MYDB.objstore.name, students);

  console.log('******************add重复添加**************************');
  INDEXDB.addData(MYDB.db , MYDB.objstore.name, students);

  console.log('******************put重复添加**************************');
  INDEXDB.putData(MYDB.db , MYDB.objstore.name, students);

  console.log('******************获取数据1001*************************');
  INDEXDB.getDataByKey(MYDB.db , MYDB.objstore.name,1001);

  console.log('******************删除数据1001*************************');
  INDEXDB.deleteData(MYDB.db , MYDB.objstore.name, 1001);

  console.log('******************删除全部数据*************************');
  INDEXDB.clearData(MYDB.db , MYDB.objstore.name);

  console.log('******************关闭数据库***************************');
  INDEXDB.closeDB(MYDB.db );

  console.log('******************删除数据库***************************');
  INDEXDB.deleteDB(MYDB.name);

}

run();
```