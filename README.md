# Evgeniia_Laba8
Зубова Евгения 803в1
-------------------------
Лаборатторная работа 8
------------------------
1)Для работы с форматом json добавляем пакет com.google.code.gson в файл guild.gradle, в секцию dependencies
![image](https://user-images.githubusercontent.com/73265788/145704865-07d22cb2-e43b-4cbc-b3a9-2b6553224280.png)


2)Для работы с json добавляем класс JSONHelper (с метанита)



                 import android.content.Context;

                 import com.google.gson.Gson;

                 import java.io.FileInputStream;
                 import java.io.FileOutputStream;
                 import java.io.IOException;
                 import java.io.InputStreamReader;
                 import java.util.List;

     public class JSONHelper {

    private static final String FILE_NAME = "data.json";

    static boolean exportToJSON(Context context, List<Library> dataList) {

        Gson gson = new Gson();
        DataItems dataItems = new DataItems();
        dataItems.setBooks(dataList);
        String jsonString = gson.toJson(dataItems);

        try(FileOutputStream fileOutputStream =
                    context.openFileOutput(FILE_NAME, Context.MODE_PRIVATE)) {
            fileOutputStream.write(jsonString.getBytes());
            return true;
        } catch (Exception e) {
            e.printStackTrace();
        }

        return false;
    }

    static List<Library> importFromJSON(Context context) {

        try(FileInputStream fileInputStream = context.openFileInput(FILE_NAME);
            InputStreamReader streamReader = new InputStreamReader(fileInputStream)){

            Gson gson = new Gson();
            DataItems dataItems = gson.fromJson(streamReader, DataItems.class);
            return dataItems.getBooks();
        }
        catch (IOException ex){
            ex.printStackTrace();
        }
        return null;
    }

    private static class DataItems {
        private List<Library> books;
        List<Library> getBooks() {
            return books;
        }
        void setBooks(List<Library> books) {
            this.books = books;
        }
    }
}


 
 

 3)Добавялем Java класс Library (библеотека



    public class Library {

    private String Name;
    private String author;
    private String data;

    Library(String name, String author, String data){

        this.Name = name;

        this.author = author;

        this.data = data;
    }
    public String getName() {
        return Name;
    }
    public void setName(String Name) {
        this.Name = Name;
    }

    public String getAuthor() {
        return author;
    }
    public void setAuthor(String author) {
        this.author = author;
    }

    public String getData() {
        return data;
    }
    public void setData(String data) {
        this.data = data;
    }

    @Override
    public String toString()
    {
        return "Название: " + Name + "\n Автор: " + author + "\n Дата: " + data;
    }
} 

 
 

 4)В MainActivity пишем код и добавим обработчики кнопок

     import android.os.Bundle;
     import android.view.View;
     import android.widget.ArrayAdapter;
     import android.widget.EditText;
     import android.widget.ListView;
     import android.widget.Toast;

    import androidx.appcompat.app.AppCompatActivity;

     import java.util.ArrayList;
     import java.util.List;

     public class MainActivity extends AppCompatActivity {

    private ArrayAdapter<Library> adapter;
    private EditText nameText, authorText, dataText;
    private List<Library> library;
    ListView listView;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        nameText = findViewById(R.id.nameText);
        authorText = findViewById(R.id.authorText);
        dataText = findViewById(R.id.dataText);
        listView = findViewById(R.id.list);
        library = new ArrayList<>();

        adapter = new ArrayAdapter<>(this, android.R.layout.simple_list_item_1, library);
        listView.setAdapter(adapter);
    }

    public void addAuthor(View view){
        String name = nameText.getText().toString();
        String author = authorText.getText().toString();
        String data = dataText.getText().toString();
        Library book = new Library(name, author, data);
        library.add(book);
        adapter.notifyDataSetChanged();
    }

    public void save(View view){

        boolean result = JSONHelper.exportToJSON(this, library);
        if(result){
            Toast.makeText(this, "Данные сохранены", Toast.LENGTH_LONG).show();
        }
        else{
            Toast.makeText(this, "Данные не сохранены", Toast.LENGTH_LONG).show();
        }
    }
    public void open(View view){
        library = JSONHelper.importFromJSON(this);
        if(library!=null){
            adapter = new ArrayAdapter<>(this, android.R.layout.simple_list_item_1, library);
            listView.setAdapter(adapter);
            Toast.makeText(this, "Данные восстановлены", Toast.LENGTH_LONG).show();
        }
        else{
            Toast.makeText(this, "Данные не открыты", Toast.LENGTH_LONG).show();
        }
    }
}

 



-------------------------------------------------
5)Проверяем работоспособность приложения:
 -------------------------------------------------
![image](https://user-images.githubusercontent.com/73265788/145704944-a845702a-4bf0-414a-95b8-1f3ba593a995.png)
 
 
![image](https://user-images.githubusercontent.com/73265788/145704951-84844441-f0eb-4e40-9c78-91e2fd630b8e.png)


