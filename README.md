# DateCalculator
Used to calculate the duration of two dates. fancy options such as excluding custom days or weekends and such
package com.example.kennethfechter.datepicker;

import android.opengl.Visibility;
import android.support.v7.app.ActionBarActivity;
import android.os.Bundle;
import android.view.Menu;
import android.view.MenuItem;

import android.app.Activity;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.CheckBox;
import android.widget.Switch;
import android.widget.TextView;

import com.android.datetimepicker.date.DatePickerDialog;
import com.android.datetimepicker.time.RadialPickerLayout;
import com.android.datetimepicker.time.TimePickerDialog;
import java.text.DateFormat;
 import java.text.SimpleDateFormat;
 import java.util.Calendar;
import java.util.Date;
import java.util.Locale;



public class DatePickingActivity extends Activity implements DatePickerDialog.OnDateSetListener{

    private Calendar startCalendar;
    private Calendar endCalendar;
    private Calendar customCalendar;
    private DateFormat dateFormat;

    private Button startButton;
    private Button endButton;
    private Button customDateButton;
    private Button calculateButton;

    private CheckBox customDate;
    CheckBox dayCheckBox = (CheckBox) findViewById(R.id.DaysCheckBox);
    CheckBox yearCheckBox = (CheckBox) findViewById(R.id.YearCheckbox);
    CheckBox saturdayCheckbox = (CheckBox) findViewById(R.id.Saturdays);
    CheckBox sundayCheckbox = (CheckBox) findViewById(R.id.Sundays);
    CheckBox customdateCheckbox = (CheckBox) findViewById(R.id.CustomDate);



    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_date_picking);

        startButton = (Button) findViewById(R.id.btnDatePickerStart);
        endButton = (Button) findViewById(R.id.btnDatePickerEnd);
        customDateButton = (Button) findViewById(R.id.customDatebtn);
        calculateButton = (Button) findViewById(R.id.calculateButton);
        calculateButton.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                CalculateInterval();
            }
        });
        endCalendar = customCalendar = startCalendar = Calendar.getInstance();
        dateFormat = DateFormat.getDateInstance(DateFormat.LONG, Locale.getDefault());
        customDateButton.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                DatePickerDialog.newInstance(new DatePickerDialog.OnDateSetListener() {
                    @Override
                    public void onDateSet(DatePickerDialog datePickerDialog, int year, int monthOfYear, int dayOfMonth) {
                        customCalendar.set(year, monthOfYear, dayOfMonth);
                        customDateButton.setText(dateFormat.format(customCalendar.getTime()));

                    }
                }, customCalendar.get(Calendar.YEAR), customCalendar.get(Calendar.MONTH), customCalendar.get(Calendar.DAY_OF_MONTH)).show(getFragmentManager(), "datePicker");            }
        });

        customDate = (CheckBox) findViewById(R.id.CustomDate);
        customDate.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                if (((CheckBox)v).isChecked())
                {
                    customDateButton.setVisibility(View.VISIBLE);
                }
                else
                {
                    customDateButton.setVisibility(View.INVISIBLE);
                }
            }
        });

        startButton.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                DatePickerDialog.newInstance(new DatePickerDialog.OnDateSetListener() {
                    @Override
                    public void onDateSet(DatePickerDialog datePickerDialog, int year, int monthOfYear, int dayOfMonth) {
                        startCalendar.set(year, monthOfYear, dayOfMonth);
                        startButton.setText(dateFormat.format(startCalendar.getTime()));

                    }
                }, startCalendar.get(Calendar.YEAR), startCalendar.get(Calendar.MONTH), startCalendar.get(Calendar.DAY_OF_MONTH)).show(getFragmentManager(), "datePicker");            }
        });

        endButton.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                DatePickerDialog.newInstance(new DatePickerDialog.OnDateSetListener() {
                    @Override
                    public void onDateSet(DatePickerDialog datePickerDialog, int year, int monthOfYear, int dayOfMonth) {
                        endCalendar.set(year, monthOfYear, dayOfMonth);
                        endButton.setText(dateFormat.format(endCalendar.getTime()));
                    }
                }, endCalendar.get(Calendar.YEAR), endCalendar.get(Calendar.MONTH), endCalendar.get(Calendar.DAY_OF_MONTH)).show(getFragmentManager(), "datePicker");
            }
        });
    }

    @Override
    public boolean onCreateOptionsMenu(Menu menu) {
        // Inflate the menu; this adds items to the action bar if it is present.
        getMenuInflater().inflate(R.menu.menu_date_picking, menu);
        return true;
    }

    @Override
    public void onDateSet(DatePickerDialog dialog, int year, int monthOfYear, int dayOfMonth) {
    }

    @Override
    public boolean onOptionsItemSelected(MenuItem item) {
        // Handle action bar item clicks here. The action bar will
        // automatically handle clicks on the Home/Up button, so long
        // as you specify a parent activity in AndroidManifest.xml.
        int id = item.getItemId();

        //noinspection SimplifiableIfStatement
        if (id == R.id.action_settings) {
            return true;
        }

        return super.onOptionsItemSelected(item);
    }

    public int findNumberOfSaturdays(){
        int totalSaturdays = 0;

        Calendar startCal = Calendar.getInstance();
        startCal.setTime(startCalendar.getTime());

        Calendar endCal = Calendar.getInstance();
        endCal.setTime(endCalendar.getTime());

        if(startCal.getTimeInMillis() == endCal.getTimeInMillis()){
            return 0;
        }

        if(startCal.getTimeInMillis() > endCal.getTimeInMillis()){
            startCal.setTime(startCalendar.getTime());
            endCal.setTime(endCalendar.getTime());
        }

        do{
            startCal.add(Calendar.DAY_OF_MONTH, 1);
            if(startCal.get(Calendar.DAY_OF_WEEK) == Calendar.SATURDAY){
                ++totalSaturdays;
            }
        }
        while(startCalendar.getTimeInMillis() < endCal.getTimeInMillis());

        return totalSaturdays;
    }

    public int findNumberOfSunday(){
        int totalSundays = 0;

        Calendar startCal = Calendar.getInstance();
        startCal.setTime(startCalendar.getTime());

        Calendar endCal = Calendar.getInstance();
        endCal.setTime(endCalendar.getTime());

        if(startCal.getTimeInMillis() == endCal.getTimeInMillis()){
            return 0;
        }

        if(startCal.getTimeInMillis() > endCal.getTimeInMillis()){
            startCal.setTime(startCalendar.getTime());
            endCal.setTime(endCalendar.getTime());
        }

        do{
            startCal.add(Calendar.DAY_OF_MONTH, 1);
            if(startCal.get(Calendar.DAY_OF_WEEK) == Calendar.SATURDAY){
                ++totalSundays;
            }
        }
        while(startCalendar.getTimeInMillis() < endCal.getTimeInMillis());

        return totalSundays;
    }

    public int weekdays(){
        return findNumberOfSunday() + findNumberOfSaturdays();
    }

    public int totalWorkdays(){
        return  totalNormalDays() - weekdays();
    }

    public int totalWorkyears(){
        return (int)(totalWorkdays()/365);
    }

    public int totalNormalDays(){
        int days;
        long difference;

        difference = endCalendar.getTimeInMillis() - startCalendar.getTimeInMillis();
        days = (int)(difference/1000*60*60*24);

        return days;
    }

    public int totalNormalYears(){
        return  (int)(totalNormalDays()/365);
    }

    public String normalYearAndDateFormat(){
        return totalNormalYears() + " Years and " + totalNormalDays()%365 + " Days.";
    }

    public String workYearAndDateFormat(){
        return totalWorkyears() + " Years and " + totalWorkdays()%365 + " Days.";
    }

    public String CalculateInterval(){
        if(dayCheckBox.isChecked() && !saturdayCheckbox.isChecked() && !sundayCheckbox.isChecked() && !customdateCheckbox.isChecked()){
            return totalNormalDays() + " Days between " + startCalendar.toString() + " and " + endCalendar.toString();
        }
        if(saturdayCheckbox.isChecked()&& dayCheckBox.isChecked()){
            return "returning day-satcheckbox";
        }
        if(sundayCheckbox.isChecked()&& dayCheckBox.isChecked()) {
            return "returning day-suncheckbox";
        }
        if(customdateCheckbox.isChecked()&& dayCheckBox.isChecked()){
            return  "returning day-customdatecheckbox";
        }

        if(yearCheckBox.isChecked() && !saturdayCheckbox.isChecked() && !sundayCheckbox.isChecked() && !customdateCheckbox.isChecked()){
            return "returning satcheckbox";}
        if(saturdayCheckbox.isChecked() && yearCheckBox.isChecked()){
            return "returning year-satcheckbox";
        }
        if(sundayCheckbox.isChecked() && yearCheckBox.isChecked()){
            return "returning year-satcheckbox";
        }
        if(customdateCheckbox.isChecked() && yearCheckBox.isChecked()){
            return "returning year-satcheckbox";
        }

        if(dayCheckBox.isChecked() && yearCheckBox.isChecked() && !saturdayCheckbox.isChecked() && !sundayCheckbox.isChecked() && !customdateCheckbox.isChecked()) {
            return "returning dayandyear";}
        if (saturdayCheckbox.isChecked() && dayCheckBox.isChecked() && yearCheckBox.isChecked()) {
            return "returning dayandyear-saturdaycheckbox";
        }
        if (sundayCheckbox.isChecked() && dayCheckBox.isChecked() && yearCheckBox.isChecked()) {
            return "returning dayandyear-sundaycheckbox";
        }
        if (customdateCheckbox.isChecked() && dayCheckBox.isChecked() && yearCheckBox.isChecked()) {
            return "returning dayandyear-cusomdatecheckbox";
        }
        return "No criteria matched.";
    }

}
