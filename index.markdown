``` dataviewjs
dv.span("") /* optional  */
const calendarData = {
    year: 2023,  // (optional) defaults to current year
    colors: { 
        gray: ["#dbdbdb", "#ebedf0", "#428bff", "#1872ff", "#0058e2"],
        blue: ["#8cb9ff", "#69a3ff", "#428bff", "#1872ff", "#0058e2"], 
        green: ["#c6e48b", "#7bc96f", "#49af5d", "#2e8840", "#196127"],
        red: ["#ff9e82", "#ff7b55", "#ff4d1a", "#e73400", "#bd2a00"],
        orange: ["#ffa244", "#fd7f00", "#dd6f00", "#bf6000", "#9b4e00"],
        pink: ["#ff96cb", "#ff70b8", "#ff3a9d", "#ee0077", "#c30062"],
        orangeToRed: ["#ffdf04", "#ffbe04", "#ff9a03", "#ff6d02", "#ff2c01"]
    },
    showCurrentDayBorder: true, // (optional) defaults to true
    defaultEntryIntensity: 1,   // (optional) defaults to 4
    intensityScaleStart: 1,    // (optional) defaults to lowest value passed to entries.intensity
    intensityScaleEnd: 6,     // (optional) defaults to highest value passed to entries.intensity
    entries: [],                // (required) populated in the DataviewJS loop below
}

//DataviewJS loop
for (let page of dv.pages('"Trips"').where(p => p.code)) {
    //dv.span("<br>" + page.destination) // uncomment for troubleshooting
    const startDate = new Date(page.startDate);
    const endDate = new Date(page.endDate);
    let currentDate = startDate;
    const oneDayInMillis = 24 * 60 * 60 * 1000;
    while (currentDate <= endDate) {
        const day = new Date(currentDate.getTime() + oneDayInMillis);
        calendarData.entries.push({
            date: day.toISOString().split('T')[0], 
            intensity: page.code, 
            color: page.color,
            content: page.content,
            destination: page.destination,
        });
        
        currentDate.setDate(currentDate.getDate() + 1);
    }
}

renderHeatmapCalendar(this.container, calendarData)
```
