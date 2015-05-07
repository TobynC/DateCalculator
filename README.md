# DateCalculator
Used to calculate the duration of two dates. fancy options such as excluding custom days or weekends and such
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

    public Date CalculateInterval(){
        Date CalculateInterval = null;

        /*This method was just to declare the method that occurs when the calculate button is pressed
        so you will probably have to change the method to a long or int or keep it as a date, however you are doing it.
        */

        return CalculateInterval;
    }
