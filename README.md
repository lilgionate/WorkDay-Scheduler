# WorkDay-Scheduler



var today = dayjs().format("dddd, MMMM DD YYYY ");

var now = dayjs().format("D A");

// current day

$("#currentDay").text(today);


/* planWorkday entries for each hour of the workday */

var planWorkday = [
    { time: "9 AM", 
        event: "" },
    { time: "10 AM", 
        event: "" },
    { time: "11 AM", 
        event: "" },
    { time: "12 PM", 
        event: "" },
    { time: "1 PM", 
        event: "" },
    { time: "2 PM", 
        event: "" },
    { time: "3 PM", 
        event: "" },
    { time: "4 PM", 
        event: "" },
    { time: "5 PM", 
        event: "" },
  ];

/* Local Storage check */
var workEvents = JSON.parse(localStorage.getItem("workDay"));
if (workEvents) {
  planWorkday = workEvents;
}

/* Current Day */
$("#currentDay").text(today);

/* Create rows */
planWorkday.forEach(function(timeBlock, index) {
	var timeLabel = timeBlock.time;
	var blockColor = colorRow(timeLabel);
	var row =
		'<div class="time-block" id="' +
		index +
		'"><div class="row no-gutters input-group"><div class="col-sm col-lg-1 input-group-prepend hour justify-content-sm-end pr-3 pt-3">' +
		timeLabel +
		'</div><textarea class="form-control ' +
		blockColor +
		'">' +
		timeBlock.event +
		'</textarea><div class="col-sm col-lg-1 input-group-append"><button class="saveBtn btn-block custom-button" type="submit"><i class="fas fa-save"></i></button></div></div></div>';

	/* Adding rows to container div */
	$(".container").append(row);
});

/* Color rows based on current time */
function colorRow(time) {
	var planNow = dayjs().hour();
	var planEntry = dayjs(time, "h A");
	if (planNow < 5 ){;
		return "future";
	} else if (planNow > 5) {
		return "past";
	} else {
		return "present";
	}
}

/* Save Events */
$(".saveBtn").on("click", function() {
	var blockID = parseInt(
		$(this)
			.closest(".time-block")
			.attr("id")
	);
	var userEntry = $.trim(
		$(this)
			.parent()
			.siblings("textarea")
			.val()
	);
	planWorkday[blockID].event = userEntry;

	/* Set local storage */
	localStorage.setItem("workDay", JSON.stringify(planWorkday));
});
