# Razor
* quick start to [Razor syntax](https://learn.microsoft.com/en-us/aspnet/core/mvc/views/razor?view=aspnetcore-6.0)
* Razor is part of the ASP.NET framework
* Razor is typically used in server-side rendered web applications, but Razor components can also be hosted in client/hybrid apps with Blazor
* Razor is compared to (and is similar to) ASP.NET MVC
	* MVC uses controller routing to handle page requests -> directs to the view engine, hydrating a model and generating a view that is returned to the client
	* Razor is very similar except request routing determines which page is generated -> directs to the view engine, hydrating a model and generating a view that is returned to the client
	* Razor content is centralized in `/Pages` vs spread out over multiple folders
	* Razor is more page-focused vs controller heavy
	* anti-forgery token validation enabled automatically on Razor pages
	* hard to create complex Razor routes
* Razor is also used in Razor Pages and Blazor, which differs from MVC, using the MVVM pattern with no controller classes

## Concepts
* **partial**: a reusable piece of Razor code that can be included in multiple pages
* **directives** dictate the setup of our page. since razor pages include a combination of html and csharp, **directives** also dictate what is happening in each part of the file


## Components
* Razor consists of two basic components: a **page** and a **model**
	* pages typically consists of a `.cshtml` page which is then rendered to the html displayed in the browser
	* each page typically has a corresponding `Model` with the name `<PageView>.cshtml.cs` with name `<PageView>Model` and is in the same namespace as the page -- note that microsoft docs specify `Index.cshtml.cs` as the model file name but that seems silly... `<PageView>Model.cs is more common`
* the separation of pages and models keeps the business logic separate from the presentation. the page model sends requests to the page and provides the data used to render the page -- this aids DI and unit testing
* the runtime looks for pages in the `/Pages` folder by default -- `index` is default for `/`
* static assets are in `/wwwroot` and are automatically served/available to the razor page
* pages can be interwoven together -- `Index.cshtml` will be wrapped by `_Layout.cshtml` and may include **partials**

## Handler Methods
* the model defines several default HTTP verbs needed to provide data to/from the page
* `OnGet` handles state initialization needed for the page
* `OnPost`/`OnPostAsync` handles actions such as form submission
* handlers return `PageResult` by default -- returning void will render the page
	* derivatives of `PageResult` include `Page` (like MVC View) and `RedirectToPageResult`

## Concepts and Syntax

## Data Binding
* we have several different types of binding
	* **one-way binding**: get only: loading data that is displayed
	* **two-way binding**: get/set: provide values that are modified or receive input (form)
	* **event binding**: binding to user actions like clicks

### Directives
* `@page`: makes the file an MVC action -- it handles requests directly without going through a controller
	* must be the first directive in a file
* `@model` specifies which model the page is bound to
* `@{...}`: razor code block containing c# code
* `@if(){...}`: conditionally render


## Styling
* **bootstrap** is commonly used with Razor


## Razor Pages
* MVC primitives like **model binding**, **validation** and **action results** work the same with razor pages as they do controllers


```csharp
// /Pages/Customers/Create.cshtml.cs
public class CreateModel : PageModel
{
    private readonly Data.CustomerDbContext _context;

	[BindProperty]
    public Customer? Customer { get; set; }

    public CreateModel(Data.CustomerDbContext context) // DI
    {
        _context = context;
    }

    public IActionResult OnGet() // HTTP get handler for MVC action result
    {
        return Page();
    }

    public async Task<IActionResult> OnPostAsync() // HTTP post handler for form submission
    {
        if (!ModelState.IsValid) // check for validation errors. in many cases, validation errors would be caught on the client
        {
            return Page(); // Page helper method returns instance of PageResult, similar to a controller returning View
        }

        if (Customer != null) _context.Customer.Add(Customer);
        await _context.SaveChangesAsync();

        return RedirectToPage("./Index"); // returns RedirectToPageResult
    }
}

// /Pages/Customers/Create.cshtml
@page
@model RazorPagesContacts.Pages.Customers.CreateModel
@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers

<p>Enter a customer name:</p>

<form method="post"> // how does this call async method?
    Name:
    <input asp-for="Customer!.Name" />
    <input type="submit" />
</form>
```


