# Carrot Market

# Requirements:

1. NextJS
- NextJS will come with below apps:
    a. React, React-DOM, NextJS

*How do you install NextJS?*
```js
// for typescript
npx create-next-app@latest --typescript
```


2. TaileindCSS

*How do you install?*

Steps:

a. install tailwindcss, postcss, autoprefixer.
```
npm install -D tailwindcss postcss autoprefixer
```

b. initiate tailwindcss
```bash
npx tailwindcss init -p
: << 'COMMENT'
Created Tailwind CSS config file: tailwind.config.js
Created PostCSS config file: postcss.config.js
COMMENT
>>
```

c. Modify tailwind.config.js to match our configurations.

tailwind.config.js
```js
// Inside any folders of page/ and component/ you will use tailwindcss
module.exports = {
  content: [
    "./pages/**/*.{js,jsx,ts,tsx}",
    "./components/**/*.{js,jsx,ts,tsx}",
  ],
  theme: {
    extend: {},
  },
  plugins: [],
};

```

d. Change the styles/globals/css to import tailwindCSS

```js
@tailwind base;
@tailwind components;
@tailwind utilities;
```

e. Test it by running below
```bash
npm run dev
```

# Dev journal

20220805
- Added @tailwindcss/forms
```
npm i @tailwindcss/forms
```
- Modify tailwind.config.js to use it

```js
module.exports = {
  content: [
    "./pages/**/*.{js,jsx,ts,tsx}",
    "./components/**/*.{js,jsx,ts,tsx}",
  ],
  theme: {
    extend: {},
  },
  darkMode: "media", // class
  plugins: [require("@tailwindcss/forms")],
};

```

# Item Detail page

- If you were to create item pages url, you just need to create a bracket around.

# Build upload screen

- You can just add below path and it will create the path in NextJS.
Path: pages/items/upload.tsx


- File choosing technique


# Create Community page

pages/community/index
- People will go into community and see IDs. 

20220808

# Chats page
divide-y-2 to create a border below

# Chat Detail
- Create a detail page in the chat by doing pages/chat/[id].tsx
- TailwindCSS used:
- w-6
- h-6
- space-x-reverse (reverse the position)
- bottom=0 - put div in the bottom.
- absolute - container father needs to be relative.
- 

# Profile page

- Create sub pages of
bought, loved, sold

# Create live screen

- People will see the current livestream. 

# Create live

- People watch videos.

# Finish creating a live stream.

- You create livestream detail ver similar to the product. 

# Layout fix

- If the screen gets too big, it becomes too ugly - extended or stretched all the way.
- Can be controlled with the app.tsx
- You can create another div surround the curren prop and you can set
```ts
// Set max width
// 
w-full max-w-xl mx-auto
```

- Create title and navigation bar.
- Create a folder called Component/layout
components/layout:
```ts
import React from "react";
import { cls } from "../libs/utils";

interface LayoutProps {
  title?: string;
  canGoBack?: boolean;
  hasTabBar?: boolean;
  children: React.ReactNode;
}

export default function Layout({
  title,
  canGoBack,
  hasTabBar,
  children,
}: LayoutProps) {
  return (
    <div>
      <div className="bg-white w-full text-lg font-medium py-3 fixed text-gray-800 border-b top-0 justify-center flex items-center">
        {title ? <span>{title}</span> : null}
      </div>
      <div className={cls("pt-16", hasTabBar ? "pb-16" : "")}>{children}</div>
      {hasTabBar ? (
        <nav className="bg-white text-gray-800 border-t fixed bottom-0 pb-10 pt-3 flex justify-between items-center"></nav>
      ) : null}
    </div>
  );
}
```

# 2. Back-End

# 2.1. Database

- Set up database with PlanetScale
*What is Prisma?*

```
## Prisma
nodejs, typescript의 ORM
ORM은 typescript와 데이터베이스 사이의 다리 역할
우선 schema.prisma에 데이터의 모양을 알려주어야 함
Prisma가 타입을 알고 있으면 client를 생성해줌
client를 통해 타입스크립트로 데이터베이스와 직접 상호작용
Prisma studio (Visual Database Browser): Admin 패널같이 데이터를 관리할 수 있음
```
- Prisma is a Node.js and TypeScript ORM (Object Relational Mapping)
- Translator or bridge between the Typescript code and database code. 
- We do not want to write DB code but you want to write the TypeScript code and let ORM translate into the DB language. 
- Schema prisma - explains the shape of your data with types and names of data. 
- It generates client for you. It hsa all the names you need. 
Ex: Model named user
- There will be prisma.users.create() that will automatically generate users. 
- There are many autocomplete that you can utilize.
- Prisma Studio: Visual Database Browser that create Admin Panel for your database. 
- It works for PostgreSQL, MySQL, SQL Server, SQLite,MongoDB.
- PlanetScale is mySQL operable database. 

# 2.1 Prisma Setup
Steps:
1.  Install VSCODE prisma extension
https://marketplace.visualstudio.com/items?itemName=Prisma.prisma

2. Install Prisma with developer dependencies.
```js
npm i prisma -D
// install prisma with developer dependencies.
```
*Need to have nodejs version higher than 14.17*

3. Invoke prisma using npx
```
npx prisma
```

```
npx prisma init
```
Results:
- It created prisma folder inside .env.
- It created .env folder inside our application.

Instructions:
1. Set the DATABASE_URL in the .env file to point to your existing database. If your database has no tables yet, read https://pris.ly/d/getting-started
2. Set the provider of the datasource block in schema.prisma to match your database: postgresql, mysql, sqlite, sqlserver, mongodb or cockroachdb.
3. Run prisma db pull to turn your database schema into a Prisma schema.
4. Run prisma generate to generate the Prisma Client. You can then start querying your database.

We can do step 2 by going into the prisma folder. 
prisma/shema.prisma:
```js
// This is your Prisma schema file,
// learn more about it in the docs: https://pris.ly/d/prisma-schema

generator client {
  provider = "prisma-client-js"
}

datasource db {
  provider = "postgresql" -> "mysql"
  url      = env("DATABASE_URL")
}

```

Create first model
prisma/shema.prisma:
```js
model User {
  id Int @id 
  // @id: this is an id of a model. 
  phone Int? 
  // ?: make it optional
  // @unique: make it unique.
  email String? @ unique
  name String
  avatar String?
  createdAt DateTime @default(now())
  updatedAt DataTime @updatedAt

}
```
- Prisma will read this file and generate typescript client and it knows how the user look like. 
- Documentations will be there on the prisma website. 

# 2.2 PlanetScale

```
## PlanetScale

mysql과 호환(Compatible)되는 serverless 데이터베이스 플랫폼

scaling을 자동으로 해줌 ( + no vacumming, no rebalencing, no query planning, no downtime)

Vitess 오픈소스를 통해 MySQL Scaling

CLI를 통해 쉽게 데이터베이스를 다룰 수 있음

마치 Git처럼 메인 데이터베이스 이외에 Branch 데이터베이스를 사용할 수도 있음

이후 Merge를 하면 자동으로 배포가 됨
```

*What is Planet Scale?*
- The MySQL-compatible serverless database platform. 
- Serverless: We do not have to maintain, secure, update for the server. 
- This is not like AWS RDS. There you have to create a server, right size and do everyting on your own. IF there are many people connecting to the DB you have to scale yourself. 
- Serverless. It is something different. 
- there are two lines of code. 
- YOur database never goes down. 
- Vitess platform: most scalable open source databse out there. 
- It is created by Google to scale YouTube.com.
- Scaling is the hardest part. 
- One server can do a lot but when it takes millions, the challenge starts to happen. 
- Horizontal, sharding and maintain downtime and others. 
- Vitess is used by Many people like Affirm, Airb&B and others. 
- It is a database clustering system that easily scale mySQL. 
  - Horizontal Scale: Transparent horizontal sharding.
  - High Availability: Vitess's primary replicat configuration allows for seamless failoever from a primary to replica.
  - MySQL Compatible: Migrate your existing MySQL applications with minimal changes while
  - Online Schema Migrations: 
  - Free plan gives 10GB 100 million read/ 10 million write/month
  1000 connections simluatanoues
  - Automatically daily backups.
  - Community Suppor.t 

- They have nice CLI as if we are interacting with git. If you are adding a new model to your db, instead of modifying the main db, they make the branch of your db, you can modify in that db. 
- Once you sure that that works, you can migrate or merge the schema. 
- Combine with Prisma, you will notice how easy it is to build. 
- DX is incredible and it maintains DB for you. 
- Click signin and create an account > login with github. 

## 2.3 Connect to PlanetScale

- Leverage CLI to install PlanetScale. 
*How do you install PlanetScale CLI?*
Ref: https://github.com/planetscale/cli

Step 1: Install Scoop
Windows Installation:
혹시 저처럼 윈도우10 환경에서 설치를 헤매고 계시다면 scoop 깃헙보고 따라하시면 됩니다.
1. Powershell 관리자로 실행
2. https://github.com/ScoopInstaller/Install#readme
3. 위 주소에서 Prerequisites, For Admin 따라하시면 됩니다!

Step 2: Install planetscale CLI

Windows
pscale is available via scoop, and as a downloadable binary from the releases page:
```
scoop bucket add pscale https://github.com/planetscale/scoop-bucket.git
scoop install pscale mysql
```
To upgrade to the latest version:

scoop update pscale

Step 3: Confirm installation

- If everything is installed correctly, you can run the below script.

```js
pscale
/*
Output:
pscale is a CLI library for communicating with PlanetScale's API.

Usage:
  pscale [command]

Available Commands:
  audit-log      List audit logs
  auth           Login and logout via the PlanetScale API
  backup         Create, list, show, and delete branch backups
  branch         Create, delete, diff, and manage branches
  completion     Generate completion script for your shell
  connect        Create a secure connection to a database and branch for a local client
  data-imports   Create, list, and delete branch data imports
  database       Create, read, delete, and dump/restore databases
  deploy-request Create, review, diff, revert, and manage deploy requests
  help           Help about any command
  org            List, show, and switch organizations
  password       Create, list, and delete branch passwords
  region         List regions
  service-token  Create, list, and manage access for service tokens
  shell          Open a MySQL shell instance to a database and branch
  signup         Signup for a new PlanetScale account

Flags:
      --api-token string          The API token to use for authenticating against the PlanetScale API.
      --api-url string            The base URL for the PlanetScale API. (default "https://api.planetscale.com/")
      --config string             Config file (default is $HOME/.config/planetscale/pscale.yml) 
      --debug                     Enable debug mode
  -f, --format string             Show output in a specific format. Possible values: [human, json, csv] (default "human")
  -h, --help                      help for pscale
      --no-color                  Disable color output
      --service-token string      Service Token for authenticating.
      --service-token-id string   The Service Token ID for authenticating.
      --version                   Show pscale version

Use "pscale [command] --help" for more information about a command.
*/
```

- Navigate CLI

- Login to Planetscale CLI
```
pscale auth login
- confirm login
```

- View region list
```powershell
pscale region list
<#
NAME (9)                            SLUG                 ENABLED
 ----------------------------------- -------------------- ---------
  AWS us-east-1 (Northern Virginia)   us-east              Yes
  AWS us-west-2 (Oregon)              us-west              Yes
  AWS eu-west-1 (Dublin)              eu-west              Yes
  AWS ap-south-1 (Mumbai)             ap-south             Yes
  AWS ap-southeast-1 (Singapore)      ap-southeast         Yes
  AWS ap-northeast-1 (Tokyo)          ap-northeast         Yes
  AWS eu-central-1 (Frankfurt)        eu-central           Yes
  AWS ap-southeast-2 (Sydney)         aws-ap-southeast-2   Yes
  AWS sa-east-1 (Sao Paulo)           aws-sa-east-1        Yes
#>

```

- Let's create it in the us-west-2 because it is the closest location from where I am at. 

Type "pscale database" to view options

- Create database in a us-west-2 region.
```powershell
pscale database create carrot-market --region us-west
# Database carrot-market was successfully created.
```

- Modify schema.prisma > .env to point to the correct database.
- Again, people never touch the real database in a computer. They have two database. They work on the fake database and update using AWS or heroku. 
- Instead, you can have a secure tunnel between the scale and your computer. 
- You don't have to write anything to .env

```
DATABASE_URL=""
```

- Make a connection to the carrot-market
```powershell
pscale connect carrot-market
<#
Secure connection to database carrot-market and branch main is established!.

Local address to connect your application: 127.0.0.1:60762 (press ctrl-c to quit)

# connected with the planetscale server.
#>
```

- Paste the Database URL to .env
  - Copy the url and paste as below
.env:
```ps1
DATABASE_URL="mysql://127.0.0.1:60762/carrot-market"
# Format: <db type>://<url>/<name of the db>
```

fdsfddsfadfdasfdsa