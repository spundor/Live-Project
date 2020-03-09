# **Live Project**
*Check out this code I created for a live project for a theater website.*

During my last two weeks of my time at the tech academy, I worked on a team developing a full scale MVC Web App in C#. This allowed a simulated experience working in the field by utilizing daily stand-ups, Slack, Azure DevOps, etc. Although there were front end and back end stories, I chose to focus on the back end to further my knowledge of C#. I worked on several back end stories and am very proud of the work I accomplished. I learned how to work cohesively as a team and utilize the knowledge I already had to research efficiently for subjects I didn’t know as well. 

Below are descriptions and code snippets of the stories I worked on.

**Donor List Function:**

The objective for this story was to create a new view within the Admin controller called “Donor List” to display subscribers who had donated to the Theater. I used a Linq statement within the Admin controller to get this list of users. I also added sorting and filtering functionality. I created a corresponding view to display users that met the criteria filtered by the controller. 
```

 public ActionResult DonorList(string sortOrder)
        {
            ViewBag.DateSortParm = sortOrder == "Date" ? "date_desc" : "Date";
            var donor = from z in db.Subscribers
                        where z.RecentDonor == true
                        select z;
                        
            switch (sortOrder)
            {
                case "date_desc":
                    donor = donor.OrderByDescending(s => s.LastDonated);
                    break;       
            }
            
            return View(donor.ToList());
        }
        
```

**User Roles:**

The objective for this story was to automatically give the user attached to a newly created subscriber object the role level of Subscriber. There was already an instance of the database context so I only needed to create an instance of the UserManager class to add a user to the Subscriber role. The Users data was passed to a SelectList.  I used an if statement in the Create function of the Subscriber controller to check that the user didn’t already have an assigned role. 
```
// GET: Subscribers/Subscriber/Create
        public ActionResult Create()
        {
            //Pass data into SelectList to display for the user to choose which user subscription relates to
            ViewData["dbUsers"] = new SelectList(db.Users.ToList(), "Id", "UserName");
            return View();
        }
    // POST: Subscribers/Subscriber/Create
        [HttpPost]
        [ValidateAntiForgeryToken]
        public ActionResult Create([Bind(Include = "SubscriberId,CurrentSubscriber,HasRenewed,Newsletter,RecentDonor,LastDonated,LastDonationAmt, SpecialRequests,Notes")]  Subscriber subscriber)
        {
            ModelState.Remove("SubscriberPerson");
            string userId = Request.Form["dbUsers"].ToString();

            if (ModelState.IsValid)
            {
                ViewData["dbUsers"] = new SelectList(db.Users.ToList(), "Id", "UserName");
                
                subscriber.SubscriberPerson = db.Users.Find(userId);
                
                var userManager = new UserManager<ApplicationUser>(new UserStore<ApplicationUser>(db));
                if (!userManager.IsInRole(userId, "Subscriber"))
                {
                    var result = userManager.AddToRole(userId, "Subscriber");
                }
                db.Subscribers.Add(subscriber);
                db.SaveChanges();
                return RedirectToAction("Index");
            }
            return View(subscriber);
        }
```

**Create Calendar Event Modifications:**

The objective for this story was to modify the Calendar Event create page to make it more intuitive for an admin to create an event on the calendar.I created dropdown lists populated with data via ViewData & SelectList. I also used logic to create a custom validation.
```

        // GET: CalendarEvents/Create
        [Authorize(Roles = "Admin")]
        public ActionResult Create()
        {
            ViewData["Productions"] = new SelectList(db.Productions.ToList(), "ProductionId","Title");
            ViewData["RentalRequests"] = new SelectList(db.RentalRequests.ToList(),"RentalRequestId", "Company");
            return View();
        } 

        // POST: CalendarEvents/Create
        [HttpPost]
        [ValidateAntiForgeryToken]
        [Authorize(Roles = "Admin")]
        public ActionResult Create([Bind(Include = "EventId,Title,StartDate,EndDate,TicketsAvailable,ProductionId,RentalRequestId")] CalendarEvent calendarEvent)
        {
            ViewData["Productions"] = new SelectList(db.Productions.ToList(), "ProductionId", "Title");
            ViewData["RentalRequests"] = new SelectList(db.RentalRequests.ToList(), "RentalRequestId", "Company");

            if ((calendarEvent.ProductionId.HasValue) && (calendarEvent.RentalRequestId != null))
            {
                var validationMessage = "Please only choose Production or Rental Request.";
                this.ModelState.AddModelError("ProductionId", validationMessage);
                this.ModelState.AddModelError("RentalRequestId", validationMessage);
            }
            if ((!calendarEvent.ProductionId.HasValue) && (!calendarEvent.RentalRequestId.HasValue))
            {
                var validationMessage = "Please only choose a Production or Rental Request.";
                this.ModelState.AddModelError("ProductionId", validationMessage);
                this.ModelState.AddModelError("RentalRequestId", validationMessage);
            }
            if (ModelState.IsValid)
            {
```    
**Implement Admin Roles:**

The objective for this story was to add authorization statements to controllers or functions.

**Other Skills Learned:**

-  Working in a team environment & problem solving as a team even if tasks are divided
- Improving my ability to clearly state my roadblocks
- Increasing the efficiency and fluency of research 
- Increasing my tolerance for frustration 
- Confidence and fluency with Visual Studio, DevOps, Slack, and other workstation tools

