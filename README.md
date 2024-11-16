package com.example.scheduleapp;

import androidx.annotation.Nullable;
import androidx.appcompat.app.AppCompatActivity;

import android.content.Intent;
import android.net.Uri;
import android.os.Bundle;
import android.widget.Button;
import android.widget.TextView;
import android.widget.Toast;

import org.json.JSONArray;
import org.json.JSONException;
import org.json.JSONObject;

import java.io.BufferedReader;
import java.io.FileNotFoundException;
import java.io.InputStream;
import java.io.InputStreamReader;

public class MainActivity extends AppCompatActivity {

    private static final int PICK_JSON_FILE = 1;

    private Button loadFileButton;
    private TextView scheduleTextView;
    private Button switchTermButton;
    private String currentTerm = "chislitel";

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        loadFileButton = findViewById(R.id.loadFileButton);
        scheduleTextView = findViewById(R.id.scheduleTextView);
        switchTermButton = findViewById(R.id.switchTermButton);

        loadFileButton.setOnClickListener(v -> {
            Intent intent = new Intent(Intent.ACTION_GET_CONTENT);
            intent.setType("*/*");
            startActivityForResult(Intent.createChooser(intent, "Select a JSON file"), PICK_JSON_FILE);
        });

        switchTermButton.setOnClickListener(v -> {
            if (currentTerm.equals("chislitel")) {
                currentTerm = "zn";
            } else {
                currentTerm = "chislitel";
            }
            try {
                String schedule = parseSchedule(jsonObject);
                scheduleTextView.setText(schedule);
            } catch (JSONException e) {
                e.printStackTrace();
                Toast.makeText(this, "Error parsing JSON", Toast.LENGTH_SHORT).show();
            }
        });
    }

    @Override
    protected void onActivityResult(int requestCode, int resultCode, @Nullable Intent data) {
        super.onActivityResult(requestCode, resultCode, data);

        if (requestCode == PICK_JSON_FILE && resultCode == RESULT_OK) {
            try {
                Uri fileUri = data.getData();
                InputStream inputStream = getContentResolver().openInputStream(fileUri);
                BufferedReader reader = new BufferedReader(new InputStreamReader(inputStream));
                StringBuilder sb = new StringBuilder();
                String line;

                while ((line = reader.readLine()) != null) {
                    sb.append(line);
                }

                jsonObject = new JSONObject(sb.toString());
                String schedule = parseSchedule(jsonObject);
                scheduleTextView.setText(schedule);
            } catch (FileNotFoundException e) {
                e.printStackTrace();
                Toast.makeText(this, "File not found", Toast.LENGTH_SHORT).show();
            } catch (Exception e) {
                e.printStackTrace();
                Toast.makeText(this, "Error reading file", Toast.LENGTH_SHORT).show();
            }
        }
    }

    private JSONObject jsonObject;

    private String parseDaySchedule(JSONObject jsonObject, String day, String term) throws JSONException {
        StringBuilder daySchedule = new StringBuilder();
        JSONArray dayArray = jsonObject.getJSONArray(day + "_" + term);
        daySchedule.append(day).append(" ").append(term).append("\n");

        for (int i = 0; i < dayArray.length(); i++) {
            daySchedule.append(dayArray.getString(i)).append("\n");
        }

        return daySchedule.toString();
    }

    private String parseSchedule(JSONObject jsonObject) throws JSONException {
        StringBuilder schedule = new StringBuilder();
        schedule.append("Version: ").append(jsonObject.getString("version")).append("\n");
        schedule.append("Group: ").append(jsonObject.getString("group")).append("\n");
        schedule.append("Subjects: ").append(jsonObject.getJSONArray("subjects").join(", ")).append("\n");

        String[] days = {"monday", "tuesday", "wednesday", "Thursday", "Friday", "Saturday"};
        String[] terms = {"chislitel", "zn"};

        for (String day : days) {
            for (String term : terms) {
                schedule.append(parseDaySchedule(jsonObject, day, term)).append("\n");
            }
        }

        return schedule.toString();
    }
}
