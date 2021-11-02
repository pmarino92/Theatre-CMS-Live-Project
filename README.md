
# Theatre-CMS-Live-Project <a id="top"></a>
Live Project with a team of members using C#, JavaScript, HTML/CSS, Visual Studio and Agile methodologies.
# Introduction 
For my C# live project at The Tech Academy, I worked with a development team of my peers on a CMS (Content Management System) for a local theatre here in Portland, Oregon. The CMS itself was made with ASP.Net MVC and Entity Framework. I personally worked on the Rental Area portion of the CMS where I employed Bootstrap coupled with CSS for the styling and added JavaScript/C# for the functionality.
# Front-end Stories
* [Styling of Index Page](#index-page)
* [Create and Edit Pages](#create-edit)


# Back-end Stories
* [Entity Model](#entity-model)



# Entity Model-Rental History <a id="entity-model"></a>
For my first story I was assigned the task of creating an entity model for Rental History. Here is the code I came up with:
```
public class RentalHistory
{
    public int Id { get; set; }
    public bool RentalDamaged  { get; set; }
    public string DamagesIncurred { get; set; }
    public string Rental { get; set; }
}
```
From there I was able to use the pre-existing DB context and scaffold the corresponding CRUD pages relatively easily.


# Styling of the Index Page <a id="index-page"></a>
After creating my model, the next step was styling the index page. To do this I employed a number of resources which included: Bootstrap, Font Awesome, and of course CSS. This story was more difficult than I anticipated with getting everything to work accordingly, however, I am proud of the outcome.

[![index.png](https://i.postimg.cc/SRTGtf4m/index.png)](https://postimg.cc/0KSmzmr4)

Features I was able to implement:
- If a rental was damaged a font awesome red X would appear to the left and if the rental was not damaged a green check would be present. 
- If the rental damage description (third column) was too long a horizontal ellipsis would shorten the description for readability's sake.
- A vertical ellipsis containing Create, Edit, Delete functions appears upon hovering any of the rows.

Below is the code that was able to accomplish the above features: 

```
@if (item.RentalDamaged == true)
      {
        <td id="Rental_History-Index-DI_Text" class="">
          @Html.DisplayFor(modelItem => item.DamagesIncurred)

          <span id="Rental_History-Index-Vertical_Ellipse">

            <button class="btn btn-dark" type="button" id="dropdownMenuButton1" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
              <i class="fa fa-ellipsis-v" aria-hidden="true"></i>
            </button>

            <ul class="dropdown-menu" aria-labelledby="dropdownMenuButton1">
              <li> <a class="dropdown-item" href="@Url.Action("Edit", new { id = item.Id })"><i style="padding-right: 5px;" class="fa fa-edit" aria-hidden="true"></i>Edit </a></li>
              <li> <a class="dropdown-item" href="@Url.Action("Details", new { id = item.Id })"> <i style="padding-right: 5px;" class="fa fa-info" aria-hidden="true"></i>Details </a></li>
              <li><div class="dropdown-divider"></div></li>
              <li> <a class="dropdown-item" href="@Url.Action("Delete", new { id = item.Id })"><span style="color: red;"><i style="padding-right: 5px;" class="fa fa-trash" aria-hidden="true"></i>Delete</span> </a></li>
            </ul>
          </span>
        </td>
      }
      else
      {
        <td id="Rental_History-Index-DI_Text" class="text-muted">
          @Html.DisplayFor(modelItem => item.DamagesIncurred)

          <span id="Rental_History-Index-Vertical_Ellipse">

            <button class="btn btn-dark" type="button" id="dropdownMenuButton1" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
              <i class="fa fa-ellipsis-v" aria-hidden="true"></i>
            </button>

            <ul class="dropdown-menu" aria-labelledby="dropdownMenuButton1">
              <li> <a class="dropdown-item" href="@Url.Action("Edit", new { id = item.Id })"><i style="padding-right: 5px;" class="fa fa-edit" aria-hidden="true"></i>Edit </a></li>
              <li> <a class="dropdown-item" href="@Url.Action("Details", new { id = item.Id })"> <i style="padding-right: 5px;" class="fa fa-info" aria-hidden="true"></i>Details </a></li>
              <li><div class="dropdown-divider"></div></li>
              <li> <a class="dropdown-item" href="@Url.Action("Delete", new { id = item.Id })"><span style="color: red;"><i style="padding-right: 5px;" class="fa fa-trash" aria-hidden="true"></i>Delete</span> </a></li>
            </ul>
          </span>
        </td>
      }

    </tr>

  }

</table>
    
   ```
# Style Create/Edit Pages <a id="create-edit"></a>
My last and final task was to add styling to the create and edit pages based off a UI/UX mockup. This is how it turned out:

[![create.png](https://i.postimg.cc/tR3NBDwN/create.png)](https://postimg.cc/t7g6TdXs)

However, styling the form was not the end of this story. Now I had to figure out a way to update the label text of "Notes" to change to "Damages" when the checkbox in the top right was clicked.
 
 This was my solution:
 ```
 $('#RentalDamaged').click(function () {
    
    if (this.checked) {
        document.getElementById("Rental_History-Create--Switch").innerHTML = "Damages";
    }
    else {
        document.getElementById("Rental_History-Create--Switch").innerHTML = "Notes";
    }
});
 ```
 This works by having this jQuery function linked to the checkbox with the #RentalDamaged id targeted and then using a simple if/else statement to change the label text. This story was rather difficult as I didn't have much experience working with jQuery previously, however, after much online research I was happy to have found a solution.
 
 
 *Jump to [Entity Model-Rental History](#entity-model), [Styling of the Index Page](#index-page), [Top](#top)


