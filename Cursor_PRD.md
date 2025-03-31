Case study overview
I will take you through my best practice workflow end to end, so you can build your next dream app; The application I want to show case is how to build an AI analytics platform that reads thousands of reddit posts, and summarise & extract core insights; Similar to https://go.gummysearch.com/



Greg Isenberg showcased how he used Gummysearch to find startup ideas, which I highly recommend watching: https://www.youtube.com/watch?v=8vXoI7lUroQ&t=242s&ab_channel=GregIsenberg



Prep the PRD
1. Fill in MUST HAVE features
To get started, I normally will write down what are MUST HAVE features we need, then fill in basic core functionalities with template below

# Project overview
xxxx

# Core functionalities
xxxx


# Doc
xxxx


# Current file structure
xxxx


# Additional requirements
1. Project setup
   - All new components should go in /components at the root (not in the app folder) and be named like example-component.tsx unless otherwise specified
   - All new pages go in /app
   - Use the Next.js 14 app router
   - All data fetching should be done in a server component and pass the data down as props
   - Client components (useState, hooks, etc) require that 'use client' is set at the top of the file

2. Server-Side API Calls:
   - All interactions with external APIs (e.g., Reddit, OpenAI) should be performed server-side.
   - Create dedicated API routes in the `pages/api` directory for each external API interaction.
   - Client-side components should fetch data through these API routes, not directly from external APIs.

3. Environment Variables:
   - Store all sensitive information (API keys, credentials) in environment variables.
   - Use a `.env.local` file for local development and ensure it's listed in `.gitignore`.
   - For production, set environment variables in the deployment platform (e.g., Vercel).
   - Access environment variables only in server-side code or API routes.

4. Error Handling and Logging:
   - Implement comprehensive error handling in both client-side components and server-side API routes.
   - Log errors on the server-side for debugging purposes.
   - Display user-friendly error messages on the client-side.

5. Type Safety:
   - Use TypeScript interfaces for all data structures, especially API responses.
   - Avoid using `any` type; instead, define proper types for all variables and function parameters.

6. API Client Initialization:
   - Initialize API clients (e.g., Snoowrap for Reddit, OpenAI) in server-side code only.
   - Implement checks to ensure API clients are properly initialized before use.

7. Data Fetching in Components:
   - Use React hooks (e.g., `useEffect`) for data fetching in client-side components.
   - Implement loading states and error handling for all data fetching operations.

8. Next.js Configuration:
   - Utilize `next.config.mjs` for environment-specific configurations.
   - Use the `env` property in `next.config.mjs` to make environment variables available to the application.

9.  CORS and API Routes:
   - Use Next.js API routes to avoid CORS issues when interacting with external APIs.
   - Implement proper request validation in API routes.

10. Component Structure:
   - Separate concerns between client and server components.
   - Use server components for initial data fetching and pass data as props to client components.

11. Security:
    - Never expose API keys or sensitive credentials on the client-side.
    - Implement proper authentication and authorization for API routes if needed.
In our specific case, my initial doc looks something like this:

**# Project overview**
You are building a Reddit analytics platform, where users can get analytics of different sub reddits, where they can see top contents & see category of posts;

You will be using NextJS 14, shadcn, tailwind, Lucid icon

**# Core functionalities**
1. See list of available sub reddits & add new sub reddits
   1. Users can see list of available sub reddits that already created display in cards, common ones like "ollama", "openai"
   2. Users can clicking on an add reddit button, which should open a modal for users to paste in reddit url and add
   3. After users adding a new reddit, a new card should be added
2. Subreddit page
   1. Clicking on each subreddit, should goes to a reddit page
   2. With 2 tabs: "Top posts", "Themes"
3. Fetch reddit posts data in "Top posts"
    1. Under "Top posts" page, we want to display fetched reddit posts from past 24 hrs
    2. We will use snoowrap as library to fetch reddit data
    3. Each post including title, score, content, url, created_utc, num_comments
    4. Display the reddits in a table component, Sort based on num of score
4. Analyse reddit posts data in "Themes"
    1. For each post, we should send post data to OpenAI using structured output to categorise “Solution requests”, “Pain & anger”, “Advice requests”, “Money talk”; 
       1. "Solution requests": Posts where people are seeking solutions for problems
       2. "Pain & anger": Posts where people are expressing pains or anger
       3. "Advice requests": Posts where people are seeking advice
       4. "Money talk": Posts where people are talking about spending money
    2. This process needs to be ran concurrently for posts, so it will be faster 
    3. In "Themes" page, we should display each category as a card, with title, description & num of counts
    4. Clicking on the card will open side panel to display all posts under this category
5. Ability to add new cards
    1. Users should be able to add a new card
    2. After a new card is added, it should trigger the analysis again

**# Doc**
xxxx

**# Current file structure**
xxxx

**# Additional requirements**
1. Project setup
   - All new components should go in /components at the root (not in the app folder) and be named like example-component.tsx unless otherwise specified
   - All new pages go in /app
   - Use the Next.js 14 app router
   - All data fetching should be done in a server component and pass the data down as props
   - Client components (useState, hooks, etc) require that 'use client' is set at the top of the file

2. Server-Side API Calls:
   - All interactions with external APIs (e.g., Reddit, OpenAI) should be performed server-side.
   - Create dedicated API routes in the `pages/api` directory for each external API interaction.
   - Client-side components should fetch data through these API routes, not directly from external APIs.

3. Environment Variables:
   - Store all sensitive information (API keys, credentials) in environment variables.
   - Use a `.env.local` file for local development and ensure it's listed in `.gitignore`.
   - For production, set environment variables in the deployment platform (e.g., Vercel).
   - Access environment variables only in server-side code or API routes.

4. Error Handling and Logging:
   - Implement comprehensive error handling in both client-side components and server-side API routes.
   - Log errors on the server-side for debugging purposes.
   - Display user-friendly error messages on the client-side.

5. Type Safety:
   - Use TypeScript interfaces for all data structures, especially API responses.
   - Avoid using `any` type; instead, define proper types for all variables and function parameters.

6. API Client Initialization:
   - Initialize API clients (e.g., Snoowrap for Reddit, OpenAI) in server-side code only.
   - Implement checks to ensure API clients are properly initialized before use.

7. Data Fetching in Components:
   - Use React hooks (e.g., `useEffect`) for data fetching in client-side components.
   - Implement loading states and error handling for all data fetching operations.

8. Next.js Configuration:
   - Utilize `next.config.mjs` for environment-specific configurations.
   - Use the `env` property in `next.config.mjs` to make environment variables available to the application.

9.  CORS and API Routes:
   - Use Next.js API routes to avoid CORS issues when interacting with external APIs.
   - Implement proper request validation in API routes.

10. Component Structure:
   - Separate concerns between client and server components.
   - Use server components for initial data fetching and pass data as props to client components.

11. Security:
    - Never expose API keys or sensitive credentials on the client-side.
    - Implement proper authentication and authorization for API routes if needed.


2. Research & getting Cursor to build POC scripts
2.1 Search for packages

Next is we want to find right packages to use for our use cases, in our app, there are 2 main functionalities:

Fetch Reddit data

Utilise OpenAI structured Output (If you want to learn more in depth, check out this tutorial: https://www.skool.com/ai-builder-club/classroom/d51daee2?md=1378fae3367840c185e7c3d810eb2b3f)

There are a few ways you can go research & find the best package to use:

Use chatGPT to find the best package to use

Use perplexity/Google for more up to date packages to use

Go to https://www.npmjs.com/ and search for packages

Just simply ask 

"I'm building a Reddit analytics platform via next.js, what is the best package to use for fetching reddit data?"

2.2 Build POC

Once you found the packages you wanna use, I will recommend build some POC with cursor to get familiar with the package, and also make sure it delivers what you need;

Typically, what i will do is open cursor composor (Command + I), then use @ mention doc and importing the doc page you found from the package

In our case, I will add doc (https://www.npmjs.com/package/snoowrap), and ask it "Help me write a quick typescript script to fetch latest posts from a sub reddit 'openai' using Snoowrap, and I want to auth using username & password"

Then following the instruction to test the script;

You also needs to get your Reddit credential:

Go to https://www.reddit.com/prefs/apps

Click 'Create another app' 

Give any name

Choose 'Script'

Redirect uri: http://localhost:3000/

Then you will get your credentials, and code example something like below

import snoowrap from 'snoowrap';

// Define the structure for a Reddit post
interface RedditPost {
  title: string;
  content: string;
  score: number;
  numComments: number;
  createdAt: Date;
  url: string;
}

// Initialize the Snoowrap client
const r = new snoowrap({
  userAgent: 'reddit_user_agent',  // You can put whatever
  clientId: 'reddit_client_id', // Get from reddit
  clientSecret: 'reddit_client_secret', // Get from reddit
  username: 'your_username', // Your reddit user name
  password: 'your_password' // Your reddit password
});

export async function fetchRecentPosts(subreddit: string): Promise<RedditPost[]> {
  const now = Math.floor(Date.now() / 1000);
  const oneDayAgo = now - 24 * 60 * 60;

  try {
    const posts = await r.getSubreddit(subreddit).getNew({ limit: 100 });
    
    const recentPosts = posts
      .filter((post: any) => post.created_utc > oneDayAgo)
      .map((post: any) => ({
        title: post.title,
        content: post.selftext,
        score: post.score,
        numComments: post.num_comments,
        createdAt: new Date(post.created_utc * 1000),
        url: post.url
      }));

    return recentPosts;
  } catch (error) {
    console.error('Error fetching Reddit posts:', error);
    return [];
  }
}

// Usage in Subreddit Page
const posts = await fetchRecentPosts('ollama');
Then you can add this as code example to the original PRD; 

We can repeat the same process for OpenAI Structured Output feature: https://platform.openai.com/docs/guides/structured-outputs



Eventually you can add documentation to the PRD

**# Project overview**
You are building a Reddit analytics platform, where users can get analytics of different sub reddits, where they can see top contents & see category of posts;

You will be using NextJS 14, shadcn, tailwind, Lucid icon

**# Core functionalities**
1. See list of available sub reddits & add new sub reddits
   1. Users can see list of available sub reddits that already created display in cards, common ones like "ollama", "openai"
   2. Users can clicking on an add reddit button, which should open a modal for users to paste in reddit url and add
   3. After users adding a new reddit, a new card should be added
2. Subreddit page
   1. Clicking on each subreddit, should goes to a reddit page
   2. With 2 tabs: "Top posts", "Themes"
3. Fetch reddit posts data in "Top posts"
    1. Under "Top posts" page, we want to display fetched reddit posts from past 24 hrs
    2. We will use snoowrap as library to fetch reddit data
    3. Each post including title, score, content, url, created_utc, num_comments
    4. Display the reddits in a table component, Sort based on num of score
4. Analyse reddit posts data in "Themes"
    1. For each post, we should send post data to OpenAI using structured output to categorise “Solution requests”, “Pain & anger”, “Advice requests”, “Money talk”; 
       1. "Solution requests": Posts where people are seeking solutions for problems
       2. "Pain & anger": Posts where people are expressing pains or anger
       3. "Advice requests": Posts where people are seeking advice
       4. "Money talk": Posts where people are talking about spending money
    2. This process needs to be ran concurrently for posts, so it will be faster 
    3. In "Themes" page, we should display each category as a card, with title, description & num of counts
    4. Clicking on the card will open side panel to display all posts under this category
5. Ability to add new cards
    1. Users should be able to add a new card
    2. After a new card is added, it should trigger the analysis again

**# Doc**
**## Documentation of how to use Snoowrap to fetch reddit posts data**
CODE EXAMPLE:
```
import snoowrap from 'snoowrap';

// Define the structure for a Reddit post
interface RedditPost {
  title: string;
  content: string;
  score: number;
  numComments: number;
  createdAt: Date;
  url: string;
}

// Initialize the Snoowrap client
const r = new snoowrap({
  userAgent: 'reddit_user_agent',
  clientId: 'reddit_client_id',
  clientSecret: 'reddit_client_secret',
  username: 'put your username here',
  password: 'put your password here'
});

async function fetchRecentPosts(subreddit: string): Promise<RedditPost[]> {
  const now = Math.floor(Date.now() / 1000);
  const oneDayAgo = now - 24 * 60 * 60;

  try {
    const posts = await r.getSubreddit(subreddit).getNew({ limit: 100 });
    
    const recentPosts = posts
      .filter((post: any) => post.created_utc > oneDayAgo)
      .map((post: any) => ({
        title: post.title,
        content: post.selftext,
        score: post.score,
        numComments: post.num_comments,
        createdAt: new Date(post.created_utc * 1000),
        url: post.url
      }));

    return recentPosts;
  } catch (error) {
    console.error('Error fetching Reddit posts:', error);
    return [];
  }
}

// Usage
fetchRecentPosts('ollama').then((posts) => {
  console.log('Recent posts from r/ollama:', posts);
});
```

**## Documentation for openai structured output**
CODE EXAMPLE:
```
import OpenAI from "openai";
import { z } from "zod";
import { zodResponseFormat } from "openai/helpers/zod";

// Define the structure for the post category analysis using Zod
const PostCategoryAnalysisSchema = z.object({
  solutionRequests: z.boolean().describe("Posts where people are seeking solutions for problems"),
  painAndAnger: z.boolean().describe("Posts where people are expressing pains or anger"),
  adviceRequests: z.boolean().describe("Posts where people are seeking advice"),
  moneyTalk: z.boolean().describe("Posts where people are talking about spending money"),
});

// Define the structure for a Reddit post
interface RedditPost {
  title: string;
  content: string;
}

// Initialize the OpenAI client
const openai = new OpenAI({
  apiKey: 'xxxxxx'
});

async function categorizePost(post: RedditPost) {
  const categoryDescriptions = Object.entries(PostCategoryAnalysisSchema.shape).map(([key, value]) => {
    return `${key}: ${value.description}`;
  }).join('\n');

  const prompt = `
    Analyze the following Reddit post and categorize it based on these criteria:
    ${categoryDescriptions}

    Post Title: ${post.title}
    Post Content: ${post.content}

    Provide a JSON response with boolean values for each category.
  `;

  const completion = await openai.beta.chat.completions.parse({
    model: "gpt-4o-mini", // You can use a different model if preferred
    messages: [
      { role: "system", content: "You are a helpful assistant that analyzes Reddit posts." },
      { role: "user", content: prompt }
    ],
    response_format: zodResponseFormat(PostCategoryAnalysisSchema, "post_category_analysis"),
  });

  return completion.choices[0].message.parsed;
}

// Example usage
async function main() {
  const post: RedditPost = {
    title: "Need help with my new laptop",
    content: "I just bought a new laptop and I'm having trouble setting it up. Any advice?"
  };

  try {
    const analysis = await categorizePost(post);
    console.log("Post Category Analysis:", analysis);
  } catch (error) {
    console.error("Error categorizing post:", error);
  }
}

main();
```


3. Setting up project & adding current project structure
We will setup Next.js project and we can use tree package to get current project structure (Thanks for people who recommended this!)

3.1 Setup Next.JS project

Create a project on Git (Which we will use for version control & backup), you can download Github desktop app: https://github.com/

Open the project folder in cursor, open terminal, and then setup Next.JS project using Shadcn command line: https://ui.shadcn.com/docs/installation/next

npx shadcn@latest init
Just say yes to setup Next.js project, give it a name 'reddit-analytics', and then enter for all default options;

After project finish, let's use command line below to navigate to the specific project folder in cursor

cursor reddit-analytics
Then adding some components we need

npx shadcn@latest add button input dialog label sheet skeleton table tab


3.2 Install the tree package to get current project structure

sudo apt-get install tree # On Linux 
brew install tree # On macOS
For Windows, you can download it via Cygwin or install it using package managers like Chocolatey.

Run the tree command: Simply run the following in your terminal to display the file structure:

tree
However this will give you everything, which is too much, normally we will do

tree -L 2 -I "node_modules|.git"
-L 2: This limits the depth of the folder structure to 2 levels.

-I: This allows you to specify patterns to ignore. In this case:

node_modules: Ignores the node_modules folder.

.git: Ignores the Git version control folder.

3.3 Adding file structure into PRD

Below should be the final initial doc we have with all information:

**# Project overview**
You are building a Reddit analytics platform, where users can get analytics of different sub reddits, where they can see top contents & see category of posts;

You will be using NextJS 14, shadcn, tailwind, Lucid icon

**# Core functionalities**
1. See list of available sub reddits & add new sub reddits
   1. Users can see list of available sub reddits that already created display in cards, common ones like "ollama", "openai"
   2. Users can clicking on an add reddit button, which should open a modal for users to paste in reddit url and add
   3. After users adding a new reddit, a new card should be added
2. Subreddit page
   1. Clicking on each subreddit, should goes to a reddit page
   2. With 2 tabs: "Top posts", "Themes"
3. Fetch reddit posts data in "Top posts"
    1. Under "Top posts" page, we want to display fetched reddit posts from past 24 hrs
    2. We will use snoowrap as library to fetch reddit data
    3. Each post including title, score, content, url, created_utc, num_comments
    4. Display the reddits in a table component, Sort based on num of score
4. Analyse reddit posts data in "Themes"
    1. For each post, we should send post data to OpenAI using structured output to categorise “Solution requests”, “Pain & anger”, “Advice requests”, “Money talk”; 
       1. "Solution requests": Posts where people are seeking solutions for problems
       2. "Pain & anger": Posts where people are expressing pains or anger
       3. "Advice requests": Posts where people are seeking advice
       4. "Money talk": Posts where people are talking about spending money
    2. This process needs to be ran concurrently for posts, so it will be faster 
    3. In "Themes" page, we should display each category as a card, with title, description & num of counts
    4. Clicking on the card will open side panel to display all posts under this category

**# Doc**
**## Documentation of how to use Snoowrap to fetch reddit posts data**
CODE EXAMPLE:
```
import snoowrap from 'snoowrap';

// Define the structure for a Reddit post
interface RedditPost {
  title: string;
  content: string;
  score: number;
  numComments: number;
  createdAt: Date;
  url: string;
}

// Initialize the Snoowrap client
const r = new snoowrap({
  userAgent: 'reddit_user_agent',
  clientId: 'reddit_client_id',
  clientSecret: 'reddit_client_secret',
  username: 'put your username here',
  password: 'put your password here'
});

async function fetchRecentPosts(subreddit: string): Promise<RedditPost[]> {
  const now = Math.floor(Date.now() / 1000);
  const oneDayAgo = now - 24 * 60 * 60;

  try {
    const posts = await r.getSubreddit(subreddit).getNew({ limit: 100 });
    
    const recentPosts = posts
      .filter((post: any) => post.created_utc > oneDayAgo)
      .map((post: any) => ({
        title: post.title,
        content: post.selftext,
        score: post.score,
        numComments: post.num_comments,
        createdAt: new Date(post.created_utc * 1000),
        url: post.url
      }));

    return recentPosts;
  } catch (error) {
    console.error('Error fetching Reddit posts:', error);
    return [];
  }
}

// Usage
fetchRecentPosts('ollama').then((posts) => {
  console.log('Recent posts from r/ollama:', posts);
});
```

**## Documentation for openai structured output**
CODE EXAMPLE:
```
import OpenAI from "openai";
import { z } from "zod";
import { zodResponseFormat } from "openai/helpers/zod";

// Define the structure for the post category analysis using Zod
const PostCategoryAnalysisSchema = z.object({
  solutionRequests: z.boolean().describe("Posts where people are seeking solutions for problems"),
  painAndAnger: z.boolean().describe("Posts where people are expressing pains or anger"),
  adviceRequests: z.boolean().describe("Posts where people are seeking advice"),
  moneyTalk: z.boolean().describe("Posts where people are talking about spending money"),
});

// Define the structure for a Reddit post
interface RedditPost {
  title: string;
  content: string;
}

// Initialize the OpenAI client
const openai = new OpenAI({
  apiKey: 'xxxxxx'
});

async function categorizePost(post: RedditPost) {
  const categoryDescriptions = Object.entries(PostCategoryAnalysisSchema.shape).map(([key, value]) => {
    return `${key}: ${value.description}`;
  }).join('\n');

  const prompt = `
    Analyze the following Reddit post and categorize it based on these criteria:
    ${categoryDescriptions}

    Post Title: ${post.title}
    Post Content: ${post.content}

    Provide a JSON response with boolean values for each category.
  `;

  const completion = await openai.beta.chat.completions.parse({
    model: "gpt-4o-mini", // You can use a different model if preferred
    messages: [
      { role: "system", content: "You are a helpful assistant that analyzes Reddit posts." },
      { role: "user", content: prompt }
    ],
    response_format: zodResponseFormat(PostCategoryAnalysisSchema, "post_category_analysis"),
  });

  return completion.choices[0].message.parsed;
}

// Example usage
async function main() {
  const post: RedditPost = {
    title: "Need help with my new laptop",
    content: "I just bought a new laptop and I'm having trouble setting it up. Any advice?"
  };

  try {
    const analysis = await categorizePost(post);
    console.log("Post Category Analysis:", analysis);
  } catch (error) {
    console.error("Error categorizing post:", error);
  }
}

main();
```

EXAMPLE RESPONSE:
```
{
  solutionRequests: true,
  painAndAnger: false,
  adviceRequests: true,
  moneyTalk: false
}
```

# Current file structure
reddit-analytics
├── README.md
├── app
│   ├── favicon.ico
│   ├── fonts
│   ├── globals.css
│   ├── layout.tsx
│   └── page.tsx
├── components.json
├── instructions
│   └── instructions.md
├── lib
│   └── utils.ts
├── next-env.d.ts
├── next.config.mjs
├── package.json
├── postcss.config.mjs
├── tailwind.config.ts
├── tsconfig.json
└── yarn.lock


4. Use OpenAI O1 model to figure out right project structure and generate final PRD
Even though this doc is great start, we actually want to use OpenAI O1 model to plan the project in advance, design the architecture and fill in necessary details we might miss

Design final project structure

Use OpenAI O1 preview model, pasting the doc above, and adding instruction

----
Above is the project i want to build, 
How should I structure my project files? 
(try to create as few files as possible)
Actual prompt looks like below:

# Project overview
You are building a Reddit analytics platform, where users can get analytics of different sub reddits, where they can see top contents & see category of posts;

You will be using NextJS 14, shadcn, tailwind


# Core functionalities
1. See list of available sub reddits & add new sub reddits
   1. Users can see list of available sub reddits that already created display in cards, common ones like "ollama", "openai"
   2. Users can clicking on an add reddit button, which should open a modal for users to paste in reddit url and add
   3. After users adding a new reddit, a new card should be added
2. Subreddit page
   1. Clicking on each subreddit, should goes to a reddit page
   2. With 2 tabs: "Top posts", "Themes"
3. Fetch reddit posts data in "Top posts"
    1. Under "Top posts" page, we want to display fetched reddit posts from past 24 hrs
    2. Each post including title, score, content, url, created_utc, num_comments
    3. Display the reddits in a table component, Sort based on num of score
4. Analyse reddit posts data in "Themes"
    1. For each post, we should send post data to OpenAI using structured output to categorise “Solution requests”, “Pain & anger”, “Advice requests”, “News”, “Money talk”; 
    2. This process needs to be ran concurrently for posts, so it will be faster 
    3. In "Themes" page, we should display each category as a card, with title, description & num of counts
    4. Clicking on the card will open side panel to display all posts under this category
5. Ability to add new cards
    1. Users should be able to add a new card
    2. After a new card is added, it should trigger the analysis again

# Doc
## Example of using openai structured output
**EXAMPLE CODE:**
```import OpenAI from "openai";
import { z } from "zod";
import { zodResponseFormat } from "openai/helpers/zod";

const openai = new OpenAI();

const ResearchPaperExtraction = z.object({
  title: z.string(),
  authors: z.array(z.string()),
  abstract: z.string(),
  keywords: z.array(z.string()),
});

const completion = await openai.beta.chat.completions.parse({
  model: "gpt-4o-mini",
  messages: [
    { role: "system", content: "You are an expert at structured data extraction. You will be given unstructured text from a research paper and should convert it into the given structure." },
    { role: "user", content: "..." },
  ],
  response_format: zodResponseFormat(ResearchPaperExtraction, "research_paper_extraction"),
});

const research_paper = completion.choices[0].message.parsed;
```
**EXAMPLE RESPONSE:**
```
{
  "title": "Application of Quantum Algorithms in Interstellar Navigation: A New Frontier",
  "authors": [
    "Dr. Stella Voyager",
    "Dr. Nova Star",
    "Dr. Lyra Hunter"
  ],
  "abstract": "This paper investigates the utilization of quantum algorithms to improve interstellar navigation systems. By leveraging quantum superposition and entanglement, our proposed navigation system can calculate optimal travel paths through space-time anomalies more efficiently than classical methods. Experimental simulations suggest a significant reduction in travel time and fuel consumption for interstellar missions.",
  "keywords": [
    "Quantum algorithms",
    "interstellar navigation",
    "space-time anomalies",
    "quantum superposition",
    "quantum entanglement",
    "space travel"
  ]
}
```

# Current file structure
reddit-analyser-test
├── README.md
├── app
│   ├── favicon.ico
│   ├── fonts
│   ├── globals.css
│   ├── layout.tsx
│   └── page.tsx
├── components
│   └── ui
├── components.json
├── instructions
│   └── instructions.md
├── lib
│   └── utils.ts
├── next-env.d.ts
├── next.config.mjs
├── package.json
├── postcss.config.mjs
├── tailwind.config.ts
├── tsconfig.json
└── yarn.lock


```
----
Above is the project i want to build, 
How should I structure my project files? 
- All new components should go in /components at the root (not in the app folder) and be named like example-component.tsx unless otherwise specified
- All new pages go in /app
- Use the Next.js 14 app router
- All data fetching should be done in a server component and pass the data down as props
- Client components (useState, hooks, etc) require that 'use client' is set at the top of the file
If the project structure OpenAI generate looks correct, then prompt it further to flesh out detailed doc

Help me adding details to the original PRD that give clear alignment to developers who will implement the project;
- dont create actual code, just the PRD
- including file structure & additional requirements into the doc
- including all documentations provided (with both example code & responses, those are important context)
Then copy the whole doc OpenAI generated, paste in Cluade to ask it to convert the file to proper markdown file, so that you can copy paste into the final instructions.md



Below are my final instructions.md

# Product Requirements Document (PRD)

## Project Overview

We are building a Reddit analytics platform that allows users to obtain analytics for different subreddits. Users can view top content, analyze categories of posts, and add new subreddits for analysis. The platform will be developed using Next.js 14, Shadcn UI components, Tailwind CSS, and Lucide Icons.

## Core Functionalities

### 1. Subreddit Management

#### 1.1 View Available Subreddits

- **Display**: A list of available subreddits displayed as cards on the home page (app/page.tsx).
- **Default Subreddits**: Include common subreddits like ollama and openai.
- **Components**:
  - SubredditCard: Renders each subreddit with its name and basic info.

#### 1.2 Add New Subreddits

- **Add Button**: An "Add Subreddit" button on the home page opens a modal.
- **Modal Functionality**:
  - Input: Users can paste a Reddit URL or enter a subreddit name.
  - Validation: Ensure the subreddit exists before adding.
- **Post-Addition**:
  - A new SubredditCard is added to the list.
  - The state is updated to include the new subreddit.
- **Components**:
  - AddSubredditModal: Handles the addition of new subreddits.

### 2. Subreddit Detail Page

#### 2.1 Navigation

- **Routing**: Clicking on a SubredditCard navigates to /[subreddit]/page.tsx.
- **Dynamic Route**: The [subreddit] folder handles dynamic routing for different subreddits.

#### 2.2 Tabs: "Top Posts" and "Themes"

- **Tabs Component**: Allows users to switch between "Top Posts" and "Themes" views.
- **State Management**: Use local state or context to manage the active tab.

### 3. Fetching Reddit Posts ("Top Posts" Tab)

#### 3.1 Data Retrieval

- **Library**: Use Snoowrap for Reddit API interactions.
- **Data Scope**: Fetch posts from the past 24 hours.
- **Data Points**:
  - title
  - score
  - content
  - url
  - created_utc
  - num_comments

#### 3.2 Displaying Posts

- **Component**: PostsTable renders the posts in a table format.
- **Sorting**: Default sorting based on the number of upvotes (score).
- **Pagination**: Implement if necessary for more than 100 posts.

### 4. Analyzing Reddit Posts ("Themes" Tab)

#### 4.1 Post Categorization

- **Categories**:
  - Solution Requests: Seeking solutions for problems.
  - Pain & Anger: Expressing pain or anger.
  - Advice Requests: Seeking advice.
  - Money Talk: Discussing spending money.
- **Process**:
  - For each post, send data to OpenAI for categorization.
  - Use structured output to receive boolean flags for each category.
- **Concurrency**:
  - Perform API calls concurrently to improve performance.

#### 4.2 Displaying Categories

- **Components**:
  - ThemeCard: Displays each category with a title, description, and post count.
  - SidePanel: Opens upon clicking a ThemeCard, showing all posts under that category.
- **Interaction**:
  - Users can view detailed posts within each category via the SidePanel.

#### 4.3 Adding New Categories

- **Functionality**:
  - Users can add new categories via an "Add Category" button.
  - Upon addition, re-trigger the analysis to include the new category.
- **Components**:
  - AddCategoryModal: Handles the addition of new categories.
- **Data Update**:
  - Update the OpenAI prompt to include the new category.

## File Structure

The project aims for minimal files without compromising clarity.

```
reddit-analytics
├── app
│   ├── favicon.ico
│   ├── fonts
│   ├── globals.css
│   ├── layout.tsx          // Main layout with header and footer
│   ├── page.tsx            // Home page with subreddit list
│   └── [subreddit]
│       └── page.tsx        // Subreddit detail page with tabs
├── lib
│   └── utils.ts            // Utility functions for data fetching and processing
├── components              // Reusable components
│   ├── SubredditCard.tsx   // Card component for subreddits
│   ├── AddSubredditModal.tsx // Modal for adding subreddits
│   ├── Tabs.tsx            // Tabs component for navigation
│   ├── PostsTable.tsx      // Table component for displaying posts
│   ├── ThemeCard.tsx       // Card component for themes
│   ├── SidePanel.tsx       // Side panel for post details
│   └── AddCategoryModal.tsx // Modal for adding categories
├── next-env.d.ts
├── next.config.mjs
├── package.json
├── tailwind.config.ts
└── tsconfig.json
```

## Documentation and Code Examples

### 1. Fetching Reddit Posts with Snoowrap

#### Installation

```bash
npm install snoowrap
```

#### Code Example

```typescript
import snoowrap from 'snoowrap';

// Define the structure for a Reddit post
interface RedditPost {
  title: string;
  content: string;
  score: number;
  numComments: number;
  createdAt: Date;
  url: string;
}

// Initialize the Snoowrap client
const r = new snoowrap({
  userAgent: 'reddit_user_agent',
  clientId: 'reddit_client_id',
  clientSecret: 'reddit_client_secret',
  username: 'your_username',
  password: 'your_password'
});

export async function fetchRecentPosts(subreddit: string): Promise<RedditPost[]> {
  const now = Math.floor(Date.now() / 1000);
  const oneDayAgo = now - 24 * 60 * 60;

  try {
    const posts = await r.getSubreddit(subreddit).getNew({ limit: 100 });
    
    const recentPosts = posts
      .filter((post: any) => post.created_utc > oneDayAgo)
      .map((post: any) => ({
        title: post.title,
        content: post.selftext,
        score: post.score,
        numComments: post.num_comments,
        createdAt: new Date(post.created_utc * 1000),
        url: post.url
      }));

    return recentPosts;
  } catch (error) {
    console.error('Error fetching Reddit posts:', error);
    return [];
  }
}

// Usage in Subreddit Page
const posts = await fetchRecentPosts('ollama');
```

**Notes**:
- Authentication: Replace the placeholder credentials with actual Reddit API credentials.
- Error Handling: Ensure proper error handling for failed API calls.

### 2. OpenAI Structured Output for Post Categorization

#### Installation

```bash
npm install openai zod
```

#### Code Example

```typescript
import OpenAI from "openai";
import { z } from "zod";
import { zodResponseFormat } from "openai/helpers/zod";

// Define the structure for the post category analysis using Zod
const PostCategoryAnalysisSchema = z.object({
  solutionRequests: z.boolean().describe("Posts where people are seeking solutions for problems"),
  painAndAnger: z.boolean().describe("Posts where people are expressing pains or anger"),
  adviceRequests: z.boolean().describe("Posts where people are seeking advice"),
  moneyTalk: z.boolean().describe("Posts where people are talking about spending money"),
});

// Initialize the OpenAI client
const openai = new OpenAI({
  apiKey: 'your_openai_api_key'
});

export async function categorizePost(post: RedditPost) {
  const categoryDescriptions = Object.entries(PostCategoryAnalysisSchema.shape).map(([key, value]) => {
    return `${key}: ${value.description}`;
  }).join('\n');

  const prompt = `
    Analyze the following Reddit post and categorize it based on these criteria:
    ${categoryDescriptions}

    Post Title: ${post.title}
    Post Content: ${post.content}

    Provide a JSON response with boolean values for each category.
  `;

  const completion = await openai.beta.chat.completions.parse({
    model: "gpt-4-0613",
    messages: [
      { role: "system", content: "You are a helpful assistant that analyzes Reddit posts." },
      { role: "user", content: prompt }
    ],
    response_format: zodResponseFormat(PostCategoryAnalysisSchema, "post_category_analysis"),
  });

  return completion.choices[0].message.parsed;
}

// Example usage
async function analyzePosts(posts: RedditPost[]) {
  // Perform concurrent API calls
  const analysisPromises = posts.map(categorizePost);
  const analyses = await Promise.all(analysisPromises);

  // Process analyses
  // ...
}
```

#### Example Response

```json
{
  "solutionRequests": true,
  "painAndAnger": false,
  "adviceRequests": true,
  "moneyTalk": false
}
```

**Notes**:
- Concurrency: Use Promise.all to process multiple posts concurrently.
- API Limits: Be mindful of OpenAI's rate limits and error handling.
- Dynamic Categories: If new categories are added, update the PostCategoryAnalysisSchema accordingly.

### 3. Component and State Management

#### Home Page (app/page.tsx)

- **State**:
  - subreddits: Array of subreddit names.
- **Functions**:
  - addSubreddit: Adds a new subreddit to the state.
- **Components**:
  - SubredditCard
  - AddSubredditModal

#### Subreddit Page (app/[subreddit]/page.tsx)

- **Data Fetching**:
  - Fetch posts in getServerSideProps or using useEffect for client-side fetching.
- **State**:
  - posts: Array of RedditPost.
  - categories: Analysis results from OpenAI.
  - activeTab: Currently selected tab.
- **Functions**:
  - fetchPosts: Fetches recent posts using Snoowrap.
  - analyzePosts: Categorizes posts using OpenAI.
- **Components**:
  - Tabs
  - PostsTable
  - ThemeCard
  - SidePanel
  - AddCategoryModal

## Developer Alignment Details

### Authentication and API Keys

- **Reddit API**:
  - Obtain credentials from Reddit Apps.
  - Ensure secure storage of clientId, clientSecret, username, and password.
- **OpenAI API**:
  - Obtain API key from OpenAI Dashboard.
  - Securely store the apiKey.

### Concurrency and Performance

- **Concurrent Processing**:
  - Use Promise.all for concurrent API calls to OpenAI.
  - Limit concurrency if necessary to avoid rate limits.
- **Caching**:
  - Implement caching strategies for Reddit posts and analysis results if appropriate.

### Error Handling

- **API Errors**:
  - Gracefully handle errors from both Reddit and OpenAI APIs.
  - Provide user feedback in the UI for any issues.
- **Validation**:
  - Validate user inputs when adding subreddits or categories.

### Styling and UI Components

- **Tailwind CSS**:
  - Utilize Tailwind classes for styling components.
  - Ensure responsiveness across different screen sizes.
- **Shadcn UI Components**:
  - Leverage pre-built components where applicable.
- **Icons**:
  - Use Lucide Icons for consistent iconography.

### State Management

- **Local State**:
  - Use React's useState and useEffect hooks for local component state.
- **Global State**:
  - If needed, use Context API or state management libraries for global state (e.g., list of subreddits).

### Accessibility and SEO

- **Accessibility**:
  - Ensure all interactive elements are accessible via keyboard.
  - Use semantic HTML elements.
- **SEO**:
  - Optimize pages for search engines with proper meta tags.

### Security Considerations

- **API Keys**:
  - Do not expose API keys in the client-side code.
  - Use environment variables and server-side fetching where possible.
- **Input Sanitization**:
  - Sanitize user inputs to prevent XSS attacks.

### Testing

- **Unit Tests**:
  - Write unit tests for utility functions in lib/utils.ts.
- **Integration Tests**:
  - Test API interactions with mock data.
- **End-to-End Tests**:
  - Use testing frameworks like Cypress for end-to-end testing.


Plan & write PRD for cursor (Important!) - [Advanced] Cursor Advanced Workflow · AI Builder Club
