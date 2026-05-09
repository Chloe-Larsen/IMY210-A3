## Reflection
I found this project very interesting to complete. I enjoyed working with strapi and having to figure out how to use a headless cms. It was interesting to understand how it create content types and how to store relating data through strapi. It was cool to see how I could just click some check boxes to decided what kind of api endpoints exist for the content I had created. The implementation with Nuxt was also and interesting process, as although we have previously working with Nuxt, I had forgotten how to work with the framework so there was a learning curve that came with the implementation of the front end of the project. The getting of the date from the api endpoints was an easy task to compete as I am getting used to the process of data retrieval  and storage from apis. This was the first time I had implemented dockerfiles so it was an interesting it learn how files need to be situated in order to correctly implement dockerfiles. Using Git has be come second nature to me by now as it has been a requirement for other modules recently for all projects to be uploaded to GitHub therefore the requirements of the GitHub commits was particularly easy to complete.

## GitHub Link
[text](https://github.com/Chloe-Larsen/IMY210-A3)

## To compile the project the Strapi backend must be compiled before the Nuxt frontend
### Strapi backend:
``cd my-strapi-backend``
``docker build -t a3-strapi .``
``docker run -p 1337:1337 a3-strapi``

### Nuxt frontend:
``cd nuxt-frontend``
``docker build -t a3-nuxt .``
``docker run -p 3000:3000 -e NUXT_PUBLIC_STRAPI_URL=http://host.docker.internal:1337 a3-nuxt``