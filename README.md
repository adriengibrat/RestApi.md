title: The rules of REST API
author:
  name: I'm Adrien, facilitator of the Sharing REST API group
  email: a.gibrat@oodrive.com
theme: ./theme
controls: false
output: index.html

--

# REST API @ ![@](theme/img/logo.svg)

## Rules & Feedback

--

## <small>1st rule</small> You document REST API

--

> Contract between backend and clients

- MUST be human readable
- SHOULD be easy to write
- MAY use [json-schema](http://json-schema.org/implementations.html)

--

## <small>2nd rule</small> You DOCUMENT REST API

--

The Waterfall mess 

```mermaid
graph LR
  b0 -.- c1
  c1 -. feedback .- b1
  b2 -.update .- c2
  subgraph Clients
    c1(feature)
    c2(deploy)
  end
  subgraph Backend
    b0(API)
    b1(other task)
    b1 -. switch .- b2(API)
  end
```

<script>
document.currentScript.parentNode.addEventListener('mermaid', event => {
  event.target.querySelector('#subGraph0 text').setAttribute('x', '-280')
  event.target.querySelector('#subGraph1 text').setAttribute('x', '-120')
})
</script>

--

vs. Documentation First

```mermaid
graph LR
  a0 -. mock .- c1
  a0 -.- b1
  b1 -. plug .- c2
  subgraph API Doc
    a0(contract)
  end
  subgraph Clients
    c1(feature) -.- c2(deploy)
  end
  subgraph Backend
    b1(API)
  end
```

<script>
document.currentScript.parentNode.addEventListener('mermaid', event => {
  event.target.querySelector('#subGraph1 text').setAttribute('x', '-120')
  event.target.querySelector('.edgePath:nth-child(4)').remove()
})
</script>

--

## <small>3rd rule</small> If backend or clients says&nbsp;"stop", fight is over

--

> API expectations

1. Usable <small>for clients (mobile, desktop, browser)</small>
2. Maintainable <small>by backend</small>
3. Extendable <small>to features</small>

--

## <small>4th rule</small> Only two kind of <acronym title="Merge Request" lang="en">MR</acronym>

--

> Sync API & doc

<small>with a</small> branch convention

- implemented<small>/descriptive_branch_name</small>
- proposal<small>/descriptive_branch_name</small>

<small>We use</small> [Git Octopus](https://github.com/lesfurets/git-octopus) <small>for preview</small>

--

## <small>5th feedback</small> One tooling at a time

--

> Tooling review

<font>&nbsp;&nbsp;&nbsp;&nbsp;😭</font> [apiblueprint](https://apiblueprint.org) <small>& [aglio](https://github.com/danielgtaylor/aglio) sucks</small>
<font>&nbsp;&nbsp;&nbsp;&nbsp;😔</font> [raml](https://raml.org) <small>has ecosystem issues</small>
<font>&nbsp;&nbsp;&nbsp;&nbsp;😎</font> [openAPI](https://www.openapis.org) <small>& [swagger](https://swagger.io/) wins</small>

--

## <small>6th feedback</small> No GraphQL, no Gateway

--

> Considered alternatives

- [Backend For Frontend](https://samnewman.io/patterns/architectural/bff) <small>maintenance issues</small>
- [GraphQL](https://graphql.org) <small>performance concerns</small>
- [API Gateway](http://microservices.io/patterns/apigateway.html) <small>not ready yet</small>

--

## <small>7th <strike>rule</strike></small> API talks will go on<br>as long as they have to

--

> So much more...

- <small>simple and durable</small> API design 
- <small>set up</small> API versioning
- <small>importance of</small> entities consistency
- <small>usefull</small> standards <small>and tricky ones</small>
- <small>from </small> silos to [plateform](https://www.oodrive.com/products/oodrive-platform)

--

## <small>8th rule</small> If this is your first API&nbsp;meeting,<br> you HAVE to write doc

--

## Thanks

<center>© [The Rules of Fight Club](http://www.diggingforfire.net/fightclub)</center>
