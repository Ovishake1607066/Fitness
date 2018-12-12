package com.example.sherl.jsonparsing;
import android.app.ProgressDialog;
import android.os.AsyncTask;
import android.os.Bundle;
import android.support.v7.app.AppCompatActivity;
import android.util.Log;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;
import android.widget.ListAdapter;
import android.widget.ListView;
import android.widget.SimpleAdapter;
import android.widget.Toast;


import org.json.JSONArray;
import org.json.JSONException;
import org.json.JSONObject;

import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

public class MainActivity extends AppCompatActivity implements View.OnClickListener {
    private Button button ;
    private EditText edittext;

    String food="Banana";
    private String TAG = MainActivity.class.getSimpleName();

    private ProgressDialog pDialog;
    private ListView lv;
    private static String url = "http://www.json-generator.com/api/json/get/cfIOwBzcwO?indent=2";

    ArrayList<HashMap<String,List<String>>> contactList=new ArrayList<>();
    ArrayList<HashMap<String, String>> contactList2=new ArrayList<>();

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        button =findViewById(R.id.but);
        edittext=findViewById(R.id.edit);
        button.setOnClickListener(this);
        lv = (ListView) findViewById(R.id.list);
    }
    @Override
    public void onClick(View v) {
        contactList2.clear();
        food=edittext.getText().toString();
        new GetContacts().execute();
    }
    private class GetContacts extends AsyncTask<Void, Void, Void> {

        @Override
        protected void onPreExecute() {
            super.onPreExecute();
            // Showing progress dialog
            pDialog = new ProgressDialog(MainActivity.this);
            pDialog.setMessage("Please wait...");
            pDialog.setCancelable(false);
            pDialog.show();
        }
        @Override
        protected Void doInBackground(Void... arg0) {
            HttpHandler sh = new HttpHandler();
            String jsonStr = sh.makeServiceCall(url);
            Log.e(TAG, "Response from url: " + jsonStr);
            if (jsonStr != null) {
                try {
                    JSONArray ja=new JSONArray(jsonStr);
                    for(int i=0;i<ja.length();i++)
                    {
                        HashMap<String,List<String>> conc=new HashMap<>();
                        JSONObject jo=ja.getJSONObject(i);
                        String mo1=jo.getString("name");
                        List<String> list=new ArrayList<>() ;
                        String mo2=jo.getString("id");
                        list.add(mo2);
                        JSONObject jo2;
                        if(jo.has("nutrition-per-100g"))
                             jo2=jo.getJSONObject("nutrition-per-100g");
                        else
                             jo2=jo.getJSONObject("nutrition-per-100ml");
                        String mo3;
                        String mo4=jo2.getString("energy");
                        list.add(mo4);
                        if(jo2.has("sugars"))
                            mo3=jo2.getString("sugars");
                        else
                            mo3=jo2.getString("energy");
                        list.add(mo3);
                        String mo5=jo2.getString("protein");
                        list.add(mo5);
                        String mo6=jo2.getString("carbohydrate");
                        list.add(mo6);
                        String mo7=jo2.getString("fat");
                        list.add(mo7);
                        conc.put(mo1,list);
                        contactList.add(conc);
                    }
                    int check=0;
                    for(int i=0;i<contactList.size();i++)
                    {
                        if(check==1)
                            break;
                        HashMap<String,List<String>> map=new HashMap<>();
                        map=contactList.get(i);
                        for (Map.Entry<String, List<String>> entry : map.entrySet()) {
                            HashMap<String,String> hmap=new HashMap<>();
                            String key = entry.getKey();
                            if(key.equals(food))
                                check=1;
                            else
                                continue;
                            key="Food Item :\n"+key;
                            hmap.put("mb1",key);
                            hmap.put("name","Nutrition-per-100g:");
                            List<String> value = entry.getValue();
                            for(int k=0;k<value.size();k++)
                            {
                                if(k==0)
                                {
                                    String mb2=value.get(k);
                                    mb2="Food catagory :\n"+mb2;
                                    hmap.put("mb2",mb2);
                                }a
                                if(k==1)
                                {
                                    String mb3=value.get(k);
                                    mb3="Energy: "+mb3;
                                    hmap.put("mb3",mb3);
                                }
                                if(k==2)
                                {
                                    String mb4=value.get(k);
                                    mb4="Sugar: "+mb4;
                                    hmap.put("mb4",mb4);
                                }
                                if(k==3)
                                {
                                   String mb5=value.get(k);
                                   mb5="Protein: "+mb5;
                                   hmap.put("mb5",mb5);
                                }
                                if(k==4)
                                {
                                    String mb6=value.get(k);
                                    mb6="Carbohydrate: "+mb6;
                                    hmap.put("mb6",mb6);
                                }
                                if(k==5)
                                {
                                    String mb7=value.get(k);
                                    mb7="Fat :"+mb7;
                                    hmap.put("mb7",mb7);
                                }
                            }
                            contactList2.add(hmap);
                        }
                    }
                    if(check==0)
                    {
                        HashMap<String,String> hmap=new HashMap<>();
                        hmap.put("mb1","Sorry Food item not found");
                        hmap.put("mb2","");
                        hmap.put("mb3","");
                        hmap.put("mb4","");
                        hmap.put("mb5","");
                        hmap.put("mb6","");
                        hmap.put("mb7","");
                        hmap.put("name","");
                        contactList2.add(hmap);
                    }
                } catch (final JSONException e) {
                    Log.e(TAG, "Json parsing error: " + e.getMessage());
                    runOnUiThread(new Runnable() {
                        @Override
                        public void run() {
                            Toast.makeText(getApplicationContext(),
                                    "Json parsing error: " + e.getMessage(),
                                    Toast.LENGTH_LONG)
                                    .show();
                        }
                    });
                }
            } else {
                Log.e(TAG, "Couldn't get json from server.");
                runOnUiThread(new Runnable() {
                    @Override
                    public void run() {
                        Toast.makeText(getApplicationContext(),
                                "Couldn't get json from server. Check LogCat for possible errors!",
                                Toast.LENGTH_LONG)
                                .show();
                    }
                });

            }
            return null;
        }
        @Override
        protected void onPostExecute(Void result) {
            super.onPostExecute(result);
            if (pDialog.isShowing())
                pDialog.dismiss();
            ListAdapter adapter = new SimpleAdapter(
                    MainActivity.this, contactList2,
                    R.layout.list_item, new String[]{"mb1", "mb2","mb3","mb4","mb5", "mb6","mb7","name"},
                    new int[]{R.id.mb1,R.id.mb2,R.id.mb3,R.id.mb4,R.id.mb5,R.id.mb6,R.id.mb7,R.id.name});
            lv.setAdapter(adapter);
        }
    }
}
