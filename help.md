// index pagination
@page
@model IndexModel
@{
    ViewData["Title"] = "Home page";
}

<div class="text-center">
    <h1 class="display-4">Welcome</h1>
    <p>Learn about <a href="https://learn.microsoft.com/aspnet/core">building Web apps with ASP.NET Core</a>.</p>
</div>


@{
    int totalPages = 10; // Assume a total number of pages is provided here
    int currentPage = 1; // Assume the current page is set here
}

<style>
    .pagination {
        display: flex;
        list-style: none;
        padding: 0;
    }

    .pagination li {
        margin: 0 5px;
    }

    .pagination a,
    .pagination span {
        text-decoration: none;
        color: black;
        padding: 5px 10px;
        border: 1px solid #ddd;
        border-radius: 5px;
        display: block;
    }

    .pagination a.active {
        font-weight: bold;
        background-color: #007bff;
        color: white;
    }

    .pagination a:hover {
        background-color: #ddd;
    }

    .pagination span {
        background-color: transparent;
        cursor: default;
    }
</style>



<div id="pagination-container">
    <ul class="pagination">
        @for (int i = 1; i <= 3; i++)
        {
            <li>
                <a href="#" class="@(i == currentPage ? "active" : "")" data-page="@i">@i</a>
            </li>
        }
        @if (totalPages > 6)
        {
            <li>
                <span>...</span>
            </li>
        }
        @for (int i = totalPages - 2; i <= totalPages; i++)
        {
            <li>
                <a href="#" class="@(i == currentPage ? "active" : "")" data-page="@i">@i</a>
            </li>
        }
    </ul>
</div>
@*
<script>
// JavaScript code will go here
document.addEventListener("DOMContentLoaded", function () {
const paginationContainer = document.getElementById('pagination-container');
const totalPages = 10; // This should match the total pages from the server-side

// Function to update the pagination display
function updatePagination(currentPage, totalPages) {
// Clear existing pagination
paginationContainer.innerHTML = '';

// Generate pagination
const paginationList = document.createElement('ul');
paginationList.className = 'pagination';

let startPage, endPage;

if (totalPages <= 9) {
// If there are 9 or fewer pages, show all pages
startPage = 1;
endPage = totalPages;
} else {
// More than 9 pages, calculate start and end pages
if (currentPage <= 4) {
startPage = 1;
endPage = 5;
} else if (currentPage >= totalPages - 3) {
startPage = totalPages - 4;
endPage = totalPages;
} else {
startPage = currentPage - 2;
endPage = currentPage + 2;
}
}

// Add the first page and ellipsis if startPage is greater than 2
if (startPage > 1) {
paginationList.appendChild(createPageLink(1, currentPage));
if (startPage > 2) {
const ellipsis = document.createElement('li');
ellipsis.innerHTML = '<span>...</span>';
paginationList.appendChild(ellipsis);
}
}

// Generate the pages in the visible window
for (let i = startPage; i <= endPage; i++) {
paginationList.appendChild(createPageLink(i, currentPage));
}

// Add the ellipsis and last page if endPage is less than totalPages - 1
if (endPage < totalPages) {
if (endPage < totalPages - 1) {
const ellipsis = document.createElement('li');
ellipsis.innerHTML = '<span>...</span>';
paginationList.appendChild(ellipsis);
}
paginationList.appendChild(createPageLink(totalPages, currentPage));
}

// Append new pagination to the container
paginationContainer.appendChild(paginationList);
}

// Function to create a page link
function createPageLink(page, currentPage) {
const paginationItem = document.createElement('li');
const paginationLink = document.createElement('a');

paginationLink.textContent = page;
paginationLink.href = "#";
paginationLink.dataset.page = page;

if (page === currentPage) {
paginationLink.classList.add('active');
}

paginationLink.addEventListener('click', function (e) {
e.preventDefault();
updatePagination(page, totalPages);
// Logic to fetch and display new data corresponding to `page` can be added here
});

paginationItem.appendChild(paginationLink);
return paginationItem;
}

// Initial pagination rendering
updatePagination(1, totalPages);
});

</script> *@

<script>
    document.addEventListener("DOMContentLoaded", () => {
        const paginationContainer = document.getElementById('pagination-container');
        const totalPages = 100; // This should match the total pages from the server-side

        const updatePagination = (currentPage, totalPages) => {
            // Clear existing pagination
            paginationContainer.innerHTML = '';

            // Create new pagination list
            const paginationList = document.createElement('ul');
            paginationList.className = 'pagination';

            // Determine the range of pages to display
            const { startPage, endPage } = calculatePageRange(currentPage, totalPages);

            // Add the first page and leading ellipsis if necessary
            if (startPage > 1) {
                paginationList.appendChild(createPageLink(1, currentPage));
                if (startPage > 2) {
                    paginationList.appendChild(createEllipsis());
                }
            }

            // Add the range of page links
            for (let i = startPage; i <= endPage; i++) {
                paginationList.appendChild(createPageLink(i, currentPage));
            }

            // Add trailing ellipsis and the last page if necessary
            if (endPage < totalPages) {
                if (endPage < totalPages - 1) {
                    paginationList.appendChild(createEllipsis());
                }
                paginationList.appendChild(createPageLink(totalPages, currentPage));
            }

            // Append new pagination to the container
            paginationContainer.appendChild(paginationList);
        };

        const calculatePageRange = (currentPage, totalPages) => {
            let startPage, endPage;

            if (totalPages <= 9) {
                // Show all pages if there are 9 or fewer
                startPage = 1;
                endPage = totalPages;
            } else if (currentPage <= 4) {
                // Near the beginning, show first 5 pages
                startPage = 1;
                endPage = 5;
            } else if (currentPage >= totalPages - 3) {
                // Near the end, show last 5 pages
                startPage = totalPages - 4;
                endPage = totalPages;
            } else {
                // In the middle, show 2 pages on each side of the current page
                startPage = currentPage - 2;
                endPage = currentPage + 2;
            }

            return { startPage, endPage };
        };

        const createPageLink = (page, currentPage) => {
            const paginationItem = document.createElement('li');
            const paginationLink = document.createElement('a');

            paginationLink.textContent = page;
            paginationLink.href = "#";
            paginationLink.dataset.page = page;

            if (page === currentPage) {
                paginationLink.classList.add('active');
            }

            paginationLink.addEventListener('click', (e) => {
                e.preventDefault();
                updatePagination(page, totalPages);
                // Logic to fetch and display new data corresponding to `page` can be added here
            });

            paginationItem.appendChild(paginationLink);
            return paginationItem;
        };

        const createEllipsis = () => {
            const ellipsisItem = document.createElement('li');
            ellipsisItem.innerHTML = '<span>...</span>';
            return ellipsisItem;
        };

        // Initial pagination rendering
        updatePagination(1, totalPages);
    });

</script>
