# androidcode-to-delete-data-from-the-database-
// button used to delete the data we have a function delete user where we gonna use it to delete the data 
//the function delete user has an arguments or paramaters saying it needs value of the username inorder to delete the data 

val btndelete: Button =findViewById(R.id.buttonDeleteUser)
        btndelete.setOnClickListener{
            val usernameToDelete= username.text.toString()
            if(usernameToDelete.isNotEmpty()){
                deleteuser(usernameToDelete)
            }else{
                Toast.makeText(this, "please enter valid username to delete",Toast.LENGTH_SHORT).show()
            }
        }

  private fun deleteuser(usernameToDelete:String){
        val dbHelper = DatabaseHelper(this)
        val db = dbHelper.writableDatabase

        val selection = "${DatabaseContract.UserEntry.COLUMN_username} = ?"
        val selectionArgs = arrayOf(usernameToDelete)

        val deletedRows = db.delete(
            DatabaseContract.UserEntry.TABLE_NAME,// The table name to delete from
            selection,
            selectionArgs
        )
        db.close()

        if (deletedRows > 0) {
            Toast.makeText(this, "User deleted successfully", Toast.LENGTH_SHORT).show()
        } else {
            Toast.makeText(this, "User not found or delete failed", Toast.LENGTH_SHORT).show()
        }
    }
    // the function database helper is used to describe the database schema and the tables in the database inshort the database helper 
    // all the information on the database 
    class DatabaseHelper(context: Context) :
        SQLiteOpenHelper(context, DATABASE_NAME, null, DATABASE_VERSION) {

        override fun onCreate(db: SQLiteDatabase) {
            val createTable =
                "CREATE TABLE ${DatabaseContract.UserEntry.TABLE_NAME} (" +
                        "${DatabaseContract.UserEntry._ID} INTEGER PRIMARY KEY," +
                        "${DatabaseContract.UserEntry.COLUMN_username} TEXT," +
                        "${DatabaseContract.UserEntry.COLUMN_password} TEXT)"

            db.execSQL(createTable)
        }

        override fun onUpgrade(db: SQLiteDatabase, oldVersion: Int, newVersion: Int) {
            db.execSQL("DROP TABLE IF EXISTS ${DatabaseContract.UserEntry.TABLE_NAME}")
            onCreate(db)
        }

        companion object {
            //creates the database
            private const val DATABASE_VERSION = 1
            private const val DATABASE_NAME = "techbean.db"
        }
    }

    object DatabaseContract {
        object UserEntry : BaseColumns {
            const val _ID = "_id"
            const val TABLE_NAME = "users"
            const val COLUMN_username = "USERNAME"
            const val COLUMN_password = "PASSWORD"
        }
    }
}
        
