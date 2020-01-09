<template>
  <div ref="indexeddb">
    <h2>IndexedDB 本地数据库</h2>
    <p>
      <button @click="addData">添加数据</button>
    </p>
    <p>
      <button @click="getData">获取数据</button>
    </p>
    <p>
      <button @click="deleteData">删除数据</button>
    </p>
    <p>
      <button @click="putData">更新数据</button>
    </p>
    <p>
      <button @click="loopData">遍历数据</button>
    </p>
    <p>
      <button @click="getDataByName">通过索引获取数据</button>
    </p>
    <p>
      <button @click="deleteDB">删除数据库</button>
    </p>
    <ol>
      <li v-for="item of studentsList"
        :key="item.id">{{item.name}}</li>
    </ol>
  </div>
</template>

<script>
/* eslint-disable */
export default {
  name: 'indexeddb',
  data() {
    return {
      db: null,
      table: null,
      studentsList: []
    }
  },

  methods: {
    // open
    // transaction => 创建事务，参数对象仓库
    // objectStore => 对象仓库
    // openCursor
    // createObjectStore
    // contains
    // put delete add get
    // createIndex
    deleteDB() {
      window.indexedDB.deleteDatabase('db_user')
    },
    loopData() {
      const db = this.db
      const req = db.transaction(
        ['students'],
        'readonly'
      )
        .objectStore('students')
        .openCursor()
      const data = []
      req.addEventListener('success', e => {
        console.log('loopData success')
        let cursor = e.target.result
        if (cursor) {
          data.push(e.target.result.value)
          console.log(e.target.result.value)
          cursor.continue()
        } else {
          console.log(cursor)
        }
        this.studentsList = data
        console.log(data)
      })

      req.addEventListener('error', e => {
        console.log('loopData error')
      })
    },
    putData() {
      const db = this.db
      const req = db.transaction(
        ['students'],
        'readwrite'
      )
        .objectStore('students')
        .put({
          id: 3,
          name: 'putdata' + Math.random()
        })

      req.addEventListener('success', e => {
        console.log('putData success')
      })

      req.addEventListener('error', e => {
        console.log('putData error')
      })
    },
    deleteData() {
      const db = this.db
      const req = db.transaction(
        ['students'],
        'readwrite'
      )
        .objectStore('students')
        .delete(4)

      req.addEventListener('success', e => {
        console.log('deleteData success')
      })

      req.addEventListener('error', e => {
        console.log('deleteData error')
      })
    },
    getDataByName() {
      const db = this.db
      const req = db.transaction(
        ['students'],
        'readonly'
      )
        .objectStore('students')
        .index('name')
        .get('0.25727803477672406')

      req.addEventListener('success', e => {
        console.log('getDataByName success')
        console.log(e.target.result)
      })

      req.addEventListener('error', e => {
        console.log('getDataByName error')
      })
    },
    getData() {
      const db = this.db
      const req = db.transaction(
        ['students'],
        'readonly'
      )
        .objectStore('students')
        .get(1)

      console.log(req)
      req.addEventListener('success', e => {
        console.log(e.target.result)
      })

      req.addEventListener('error', e => {
        console.log(e)
      })
    },
    addData() {
      const db = this.db
      const req = db.transaction(
        ['students'],
        'readwrite'
      )
        .objectStore('students')
        .add({
          name: '' + Math.random()
        })

      req.addEventListener('success', () => {
        console.log('write success')
      })

      req.addEventListener('error', (e) => {
        console.log('write error')
        console.log(e)
      })
    },
    createDB() {
      const dbConnect = window.indexedDB.open('db_user', 10)

      dbConnect.addEventListener('error', e => {
        console.log('db error')
      })

      dbConnect.addEventListener('success', e => {
        console.log('db success')
        const db = e.target.result
        this.db = db
        console.log('---')
         // 数据库版本变化
        dbConnect.result.addEventListener('versionchange', e => {
          console.log(e.target)
        })
      })
      // 版本变更
      dbConnect.addEventListener('upgradeneeded', e => {
        console.log('db upgradeneeded')
        console.log(dbConnect)
        const db = e.target.result
        const trans = e.target.transaction
       
        if (!db.objectStoreNames.contains('students')) {
          const os = db.createObjectStore('students', {
            keyPath: 'id',
            autoIncrement: true
          })
          os.createIndex(
            'name',
            'name',
            {
              unique: true
            }
          )
          const store = trans.objectStore(
            ['students'],
            'readwrite'
          )
        }
      })
      dbConnect.addEventListener('blocked', e => {
        console.log('db blocked')
        console.log(e)
        console.log('---')
      })
    }
  },
  
  created() {
    this.createDB()
  }
}
</script>