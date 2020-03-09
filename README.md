# Live-Project
Check out this code I created for a live project for a theater website.

During my last two weeks of my time at the tech academy, I worked on a team developing a full scale MVC Web App in C#. This allowed a simulated experience working in the field by utilizing daily stand-ups, Slack, Azure DevOps, etc. Although there were front end and back end stories, I chose to focus on the back end to further my knowledge of C#. I worked on several back end stories and am very proud of the work I accomplished. I learned how to work cohesively as a team and utilize the knowledge I already had to research efficiently for subjects I didn’t know as well. 

Below are descriptions and code snippets of the stories I worked on.

Donor List Function:

The objective for this story was to create a new view within the Admin controller called “Donor List” to display subscribers who had donated to the Theater. I used a Linq statement within the Admin controller to get this list of users. I also added sorting and filtering functionality. I created a corresponding view to display users that met the criteria filtered by the controller. 


{
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
       }
}
