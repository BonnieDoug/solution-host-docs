# Solution Host Rebuild Project Overview
## Contents
1. [Introduction](#introduction)
2. [Project Organisation](#project-organisation)
3. [Managerial Process](#managerial-process)
4. [Technical Process](#technical-process)
5. [Work Packages, Schedule and Budget](#budget)
6. [Additional Components](#additional-components)
7. [Index](#index)

## <a name="introduction"></a> 1. Introduction
This section should describe the project and the software product being to be built. No text is necessary between the heading above and the heading below unless otherwise desired.
### 1.1 Project Overview
Give a short summary of the project objectives, the software to be delivered, major activities, major deliverables, major milestones, required resources, and top-level schedule and budget. Describe the relationship of this project to other projects, if appropriate.
### 1.2 Project Deliverables
List all of the major items to be delivered to the customer (external customer, in-house user, etc.).

List the deliverables, delivery dates, delivery locations, delivery method (email, FTP, CD, etc.), and quantities necessary to satisfy the project’s requirements.
### 1.3 Evolution of the Software Project Management Plan
Describe how you expect this document to evolve over time. This section should be very similar to the "Revision Chart" earlier in the document. The revision chart should list what has already been done to this document. This section should list what is expected to be done to this document.

### 1.4 Reference Materials
* [Api Platform](https://api-platform.com/) - REST Framework for Symfony
* [VueJS](https://vuejs.org/) - JavaScript framework
* [NuxtJS](https://nuxtjs.org/) - SSR Framework built for VueJS
* [Vuetify](https://vuetifyjs.com/) - VueJS Style framework (Like Bootstrap but not)
* [JWT.io](https://jwt.io/) - JSON Web Token + Token Validator for development
* [NodeJS](https://nodejs.org/) - JavaScript engine
* [Composer](https://getcomposer.org/) - PHP Dependecies manager
* [Symfony](https://symfony.com) - PHP Framework
* [NPM](https://www.npmjs.com/) - JavaScript dependencies manager
* [Heroku](https://heroku.com) - Cloud hosting
* [Cloudflare](https://cloudflare.com) - Cloud hosting CDN (for static files such as JS, Css, Images etc)
* [Dropone](http://www.dropzonejs.com/) - JavaScript drag-n-drop file uploader plugin
* [JSLint](http://www.jslint.com/) - JavaScript code quality inspector / Enforcer. 

### 1.5 Definitions and Acronyms
#### JWT
> JSON Web Token (JWT) is an open standard (RFC 7519) that defines a compact and self-contained way for securely transmitting information between parties as a JSON object. This information can be verified and trusted because it is digitally signed. JWTs can be signed using a secret (with the HMAC algorithm) or a public/private key pair using RSA.
#### REST
> REST (REpresentational State Transfer) is an architectural style for developing web services. REST is popular due to its simplicity and the fact that it builds upon existing systems and features of the internet's HTTP in order to achieve its objectives, as opposed to creating new standards, frameworks and technologies.
#### API
> A RESTful API breaks down a transaction to create a series of small modules. Each module addresses a particular underlying part of the transaction. This modularity provides developers with a lot of flexibility.

## <a name="project-organisation"></a> 2. Project Organisation
In this section, describe the process model (e.g., lifecycle model), the organizational structure (e.g., chain of command or management reporting structure), and responsibilities of individuals on the project.

No text is necessary between the heading above and the heading below unless otherwise desired.
### 2.1 Process Model
Describe the following items:
* The project’s lifecycle model (e.g., waterfall model, spiral model, evolutionary prototyping model, etc.)
* The project’s major milestones (content and dates/timing). This should include a text description of the meaning of each milestone plus a Gantt chart or other high-level description of the project’s schedule.
* Major work products, including content and timing. These should be listed in a table like the following one:
### 2.2 Organisational Structure

### 2.3 Organisational Boundaries and Interfaces
### 2.4 Project Responsibilities
## <a name="managerial-process"></a> 3. Managerial Process
### 3.1 Management Objectives and Priorities
### 3.2 Assumptions, Dependencies, and Constraints
### 3.3 Risk Management
### 3.4 Monitoring and Controlling Mechanisms
### 3.5 Staffing Plan
## <a name="technical-process"></a> 4. Technical Process
### 4.1 Methods, Tools, and Techniques
#### 4.1.1 Setup new API
Setting up a new api project involves a few steps. These steps could differ slightly from project to project however, most will be the same.

1. Setup a new **BLANK** repository on the HS-Direct GitHub. Fork this repository into your own account.
2. On your machine, in the directory you store sites, use the [Symfony Composer installer](https://symfony.com/download) to begin a fresh Symfony projectby running `composer create-project symfony/skeleton name-of-project` Once the install has completed `cd` into the new directory.
3. Run `git init` to initialise the directory/site as a git repository.
4. Run ```
        git add .
        git commit -m "Init"
        ``` To add the current files to git.
5. Add the upstream and origin branches to the directory by running
   * `git remote add origin git@github.com:YourName/Name-of-repo.git`
   * `git push -u origin master`
6. Add and Install dependencies:
   ```
   composer req api
   composer req security
   composer req server
   composer req jwt-auth
   composer req maker
   ```
7. Setup .env to point at the right database etc.
8. Decide which user authentication you need. There is, at the time of writing, 3 different User providers. FFC-Contractor / My-Task and Client. Once decided, goto a project you know is using that authentication and copy the project/config/jwt folder into your config folder. Grab the Api security section from the security.yaml and probably at this point ask Doug to help.
9. Run `php bin/console server:start` to see if all has worked correctly to this point.
10. ```
     git add .
     git commit -m "Added dependencies
     git push
     ```

### 4.2 Software Documentation
#### 4.2.1 Security API
##### To-do
The sole responsibility of the [security-api](https://github.com/HS-Direct/security-api/) is to authenticate users following a login request (JSON or Form). Once authenticated return with a JWT token response. This JWT token should then be validated on all requests for protected data. This authentication check will be processed by the API in which you are requesting e/g `sfp-api`.
###### Login requirements
- Client ID
- Username / email*
- Password *
A client ID is not necessarily required, as such we must check for non-unique username/email before we can allow login.

###### Entities
An entity is an object which describes the structure of your database tables. These allow for less repetition within the code base and standardize the way data is retrieved. We will use Doctrine ORM for this. Entities required:
- [ ] User
- [ ] Client
- [ ] AppStatus
- [ ] Privilege
- [ ] Role

###### Roles
User roles should be well defined, they will form a tree structure starting from `ROLE_READ_ONLY` to `ROLE_SUPER_ADMIN`, all authenticated users should be `ROLE_READ_ONLY`, this role should only provide the current user with reading access to any entity they own or are in some way connected to. The role hierarchy should trickle down, so a user with the maximum role should also inherit all previous roles. Entities should be secured by role.

- `ROLE_USER`  - Only read entities owned by this user, useful if write privilege has been revoked
- - `ROLE_WRITE` - Write own entities. No access to other entities, cannot delete or approve.
- - - `ROLE_SUPERVISOR` - Can read, write, retire entities owned by supervisors team. Cannot modify entities owned by users with higher privileges. Can approve entities.
- - - - `ROLE_MANAGER` - Can read, write, retire entities owned by the entire client.
- - - - - `ROLE_CLIENT_DIRECTOR` - Can read, write,  retire, delete, modify and approve all entities owned by client.
- `ROLE_ADMIN` - Replaces current admin account. Allows the user to modify, create and read entities. Cannot Delete. Limited access to certain parts, depending on rules defined by James etc.
- - `ROLE_CONSULTANT` - Access to privileges that only consultants should have access to.
- - - `ROLE_SUPER_ADMIN` - Like `ROLE_ADMIN` but with further powers. Possibly for Technical and Directors?
- -  `ROLE_STAFF` - Access to 000.
- - - `ROLE_SALES` - Access to own clip.
- - - - `ROLE_SALES_LEADER` - Access to team clips
- - - `ROLE_STAFF_ADMIN` - Access to all clips
- - - - `ROLE_STAFF_ACCOUNTS` - Access to stuff only accounts will need.
- - - - `ROLE_STAFF_TECH` - Tech access
- - - - `ROLE_STAFF_DIRECTOR` - Access to everything
- - - - - `ROLE_STAFF_ADMIN` - Further access
- - - - - - `ROLE_STAFF_SUPER_ADMIN` - Full access (James only?)

e.g `ROLE_STAFF_ADMIN` would become an array as below:
```
$role = [
    'ROLE_USER',
    'ROLE_STAFF',
    'ROLE_STAFF_ADMIN'
];
```
###### Bridge
Modify existing security stuff, in Portal, to use new Security-Api. Places requiring changes:
- [ ] Portal/web/apps/userlogin/backend.php::login() 
- [ ] Portal/web/apps/userlogin/backend.php::is_logged()
- [ ] HS-Direct/login
- [ ] EL-Direct/login

###### Firewall


###### JWT Encoder/Decoder


### 4.3 Project Support Functions
## <a name="budget"></a> 5. Work Packages, Schedule, and Budget
### 5.1 Work Packages
### 5.2 Dependencies
### 5.3 Resource Requirements
### 5.4 Budget and Resource Allocation
### 5.5 Schedule
## <a name="additional-components"></a> 6. Additional Components
## <a name="index"></a> 7. Index
## <a name="appendicies"></a> 8. Appendices
* [The template for this plan http://www.construx.com/Software_Development_Plan/](http://www.construx.com/Software_Development_Plan/)
* [Markdown syntax reference](https://en.support.wordpress.com/markdown-quick-reference/)
