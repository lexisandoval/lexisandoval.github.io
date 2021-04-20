---
layout: post
title:      "Javascript Project Fetch Request Guide"
date:       2021-04-20 20:07:12 +0000
permalink:  javascript_project_fetch_request_guide
---


I just completed my second to last Flatiron project and I cannot tell you how excited I am about the progress I have made during the Bootcamp. I can confidently say that I have learned more over the past four months than I did during the four and a half years I spent at my university. The Flatiron team has helped me immensely by teaching material in a simple yet effective way, and I believe that is the main reason I have made it to this point. 

During the Javascript project development stage, I followed along with Ayana Zaire's live project build. While the video series was **very** helpful, I still had a few issues with fetch requests as the videos did not cover the missing pieces that I needed to complete my project. So, I thought it'd be helpful to create a guide for get, post, and patch requests in case other students are stuck in the same position that I was in a few days ago.

For my project, I created an SPA that lists Chicago's best rooftop restaurants and bars (check it out [here](https://windycityrooftops.netlify.app)). For this project, I included two models: a neighborhood model which has many rooftops, and a rooftop model which belongs to a neighborhood. 

Here is a snippet from my rails API:

```
{
  "data": [
    {
      "id": "10",
      "type": "rooftop",
      "attributes": {
        "name": "Aba",
        "address": "302 N Green St",
        "image_url": "https://s3.amazonaws.com/bucket2.abarestaurants.com/wp-content/uploads/Aba_FullExterior_creditJeffMarini-1.jpg",
        "website_url": "https://www.abarestaurants.com/chicago",
        "description": "The loungey, greenery-lined setup at this West Loop restaurant makes it easy to relax—and the sights of the surrounding neighborhood don't hurt either. Order a few Medditeranian dishes from the menu (the spicy hummus, whipped feta dip or the big eye tuna carpaccio are all surefire hits) and then explore the cocktail menu. Even the drinks with the cheekiest names are worth a try—our favorite is the favorite Aloe? It's Me with mezcal, aloe, green juice, lime and jalapeño!",
        "neighborhood": {
          "id": 6,
          "name": "West Loop",
          "created_at": "2021-04-15T21:42:06.029Z",
          "updated_at": "2021-04-15T21:42:06.029Z"
        }
      }
    },
	...
	]
}
```


Here are the global variables I declared along with the DOMContentLoaded listener at the top of my index.js file:

```
const endPoint = "https://windycityrooftops-api.herokuapp.com/api/v1/rooftops"
const createRooftopForm = document.querySelector('#rt');

document.addEventListener('DOMContentLoaded', () => {

  getRooftops()

  createRooftopForm.addEventListener('submit', (e) => createFormHandler(e))

  editRooftop()
})
```


And here is what I did for each of the fetch requests:

GET:
```
function getRooftops() {
  fetch(endPoint)
  .then(response => response.json())
  .then(rooftops => {
    rooftops.data.forEach(rooftop => {
      const newRooftop = new Rooftop(rooftop, rooftop.attributes)

      document.querySelector('#rooftop-container').innerHTML += newRooftop.render()
    })
  }) 
}
```

POST:
```
// set values for add rooftop form
function createFormHandler(e) {
  e.preventDefault()

  const nameInput = document.querySelector('#rt-name').value;
  const addressInput = document.querySelector('#rt-address').value;
  const imgInput = document.querySelector('#rt-image').value;
  const websiteInput = document.querySelector('#rt-website').value;
  const descriptionInput = document.querySelector('#rt-description').value;
  const neighborhoodID = parseInt(document.querySelector('#neighborhoods').value);

  postRooftop(nameInput, addressInput, imgInput, websiteInput, descriptionInput, neighborhoodID)
}

// post new rooftop to API
function postRooftop(name, address, image_url, website_url, description, neighborhood_id) {
  const bodyData = {name, address, image_url, website_url, description, neighborhood_id}

  fetch(endPoint, {
    method: "POST",
    headers: {"Content-Type": "application/json"},
    body: JSON.stringify(bodyData)
  })
  .then(response => response.json())
  .then(rooftop => {

    const rooftopData = rooftop.data

    const newRooftop = new Rooftop(rooftopData, rooftopData.attributes)

    document.querySelector('#rooftop-container').innerHTML += newRooftop.render()

    createRooftopForm.reset();
  })
}
```

PATCH:
```
// set values for edit rooftop form
function updateFormHandler(e) {
  e.preventDefault();

  const rooftop = Rooftop.findById(e.target.id);
  const id = rooftop.id
  const name = e.target.querySelector('#rt-name').value;
  const address = e.target.querySelector('#rt-address').value;
  const image = e.target.querySelector('#rt-image').value;
  const website = e.target.querySelector('#rt-website').value;
  const description = e.target.querySelector('#rt-description').value;
  const neighborhoodID = parseInt(e.target.querySelector('#neighborhoods').value);

  patchRooftop(id, name, address, image, website, description, neighborhoodID)
}

// edit rooftop in API
function patchRooftop(id, name, address, image_url, website_url, description, neighborhood_id) {
  const bodyData = {name, address, image_url, website_url, description, neighborhood_id}

  fetch(`${endPoint}/${id}`, {
    method: 'PATCH',
    headers: {
      'Content-Type': 'application/json',
      Accept: 'application/json',
    },
    body: JSON.stringify(bodyData),
  })
    .then(response => response.json())
    .then(updatedRooftop => {
      
      console.log(updatedRooftop)
    })
}
```

*It is also very important to remember to include all the necessary attributes in your model_params helper in Rails!*

To all Flatiron students: good luck with your projects! We're nearing the finish line and we've got this!
