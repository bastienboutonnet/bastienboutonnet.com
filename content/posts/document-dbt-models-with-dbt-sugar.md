---
title: "Document dbt Models the easy way with dbt-sugar"
date: 2021-03-26T14:28:18+01:00
draft: false
---

Would you like your documentation workflow to look something like this? Now it's possible with [dbt-sugar](https://bitpicky.gitbook.io/dbt-sugar/)

![documenting a model with dbt-sugar](/img/document_fct_orders.gif)
**NOTE:** This tool is still pretty new and in alpha/beta stage, but we think you can start testing it and helping us making it better along the way.

- [Contribute on GitHub](https://github.com/bitpicky/dbt-sugar)
- Become part of the community by joining [our Discord server](https://discord.com/invite/cQB49ejbCA)

If you're a user of [dbt](getdbt.com) chances are you're already are on a great path to making your data team successful at iterating and serving analytical value to your business. If you don't, it's not too late **especially if you don't have an ELT/ETL workflow that let's your data people (be it analysts, data engineers or data scientists) only focus on writing a SQL `select` statement**.

If you don't know what dbt is, no one better than the makers of the tool can speak about it and here are places where you can **learn more about the tool**, as well as ways in which a growing number of data teams are using it across the world.

- [getdbt.com](https://www.getdbt.com/)
- [dbt documentation](https://docs.getdbt.com/)
- [git hub repo for dbt-core open-source](https://github.com/fishtown-analytics/dbt)
- [YouTube channel](https://www.youtube.com/channel/UCVpBwKK-ecMEV75y1dYLE5w)

Let's start with some context! However, if you'd like to cut to the chase and learn more about the tool itself you can jump to [this section](#what-is-dbt-sugar)

## We <3 Documentation

That's a fact, good documentation is just a gift from whatever higher-forces of data you believe in. Not just data actually. For example, in my engineering entourage, I often overhear conversations between developers that go something like "Oh my god have you ever used the API from <insert_company> their documentation is SO amazing!". By the by, [dbt](<[getdbt.com](https://docs.getdbt.com/)>) is one, [Stripe](https://stripe.com/docs/api) is another.

**The reality**, though, is we're all hiding some pain underneath our smiles and proclaimed love for documentation. Writing documentation is often, and quite sadly, an afterthought in the modern fast-paced data world. So is testing by the way but that's a topic for another time! As a consequence, instead of "I love writing documentation, it's one of the first things I do" we often hear "Yeah... I'll document this later, it's a small thing, I don't have the time now to go back and forth and update it."

### Why do we skimp so much?

Why do we skimp so much on something we all recognise the value, joy and enablement good and accurate documentation is to us?

My feeling, and one that I'm sure a lot of people share, is that writing documentation and keeping it up to date kinda sucks. It sucks because it's something else you have to do. It's not a blocker so you don't have to do it in order to make your release work. It sucks because, usually, documentation lives "somewhere else". It sucks because it's nearly impossible to keep it up to date in a predominantly incremental and iterative workflow (the _de facto modus operandi_ for most data practitioners). Even in an awesome tool like dbt, it's somewhere else and not automatically kept up to date. That being said, it's not far at all and dbt offers a great way to [serve this documentation as a webpage out of the box](https://docs.getdbt.com/docs/building-a-dbt-project/documentation) so if you use it, you're already ahead and in a much better place.

## Documenting your dbt projects and keeping it consistent is now a fun experience if you use `dbt-sugar`

### What is dbt-sugar?

> dbt-sugar is an opinionated CLI tool which helps users of dbt documenting fields and enforcing test coverage on their dbt models. It makes documentation and testing the _entitas principalis_ of data modellers' every day workflow.

If you attended [Coalesce](https://www.getdbt.com/coalesce) last December, you will have heard my colleague [Jeff](https://www.linkedin.com/in/jflairie) and I talk about the kind of tools that our team at [TripActions](https://tripactions.com/) developed to make our workflows more efficient and less human-error prone (watch the video [Supercharging your data team](https://www.youtube.com/watch?v=HFNZGZqr5QM&t=411s) on dbt's YouTube channel). One of the tools our amazing data engineer [Virginia](https://www.linkedin.com/in/virginia-l%C3%B3pez-gil-p%C3%A9rez-3b86ab101/) built is a lot like dbt-sugar --that's kinda where the inspiration for it comes from and I coincidentally happen to maintain Virginia in our downtime under the [bitpicky](https://github.com/bitpicky) open source organisation umbrella I started a few months ago.

### Ok show me what it does already!

In short, dbt-sugar will guide you through documenting a dbt model in your dbt project. You simply call the `dbt-sugar doc` task, point it to your model and _voila_ you get an experience such as the one below:

![documenting a model with dbt-sugar](/img/document_fct_orders.gif)

You go from an empty `schema.yml` to a nicely populated one like this

![automatically populated schema.yml](/img/fct_orders_schema.png)

#### **But WAIT, you only wrote documentation for the `order_date` column, how do you get the `order_id` column to be magically filled in??**

Good catch! Cool isn't it? **The main advantage of using dbt-sugar is that it knows about the columns you already documented in all of your other models and it populates it for you**!

#### It runs the tests?

Yep! No point adding a test if it fails already right? This would lead you to deploy your model, only to have the model fail its assumptions on the next run. When you ask for a test to be added on a column, dbt-sugar will run it for you. If it passes, great it gets added, if it fails you get a message that one of your assumptions isn't met and you can fix it before it ends up in the code base and no-one will ever know!

![test addition summary output](/img/test_summary_output.png)
In this case, the unique test **failed** so it was not added and `dbt-sugar` told you about this so you don't have any surprises.

### What else does dbt-sugar do for you?

- Allows you to document **undocumented columns**
- Allows you to re-document **already documented columns** and add missing tests or tags
- Gives you a nice **audit** summary. For example if you do `dbt-sugar audit` on your dbt model you will get something that looks like this:

![dbt sugar project-level audit summary](/img/dbt_sugar_audit_task.png)

## Want to start using it?

- dbt-sugar is currently able to talk to **Snowflake** and **Postgres** databases and is extensible to work for most other major databases (feel free to [get inlvoved if you have access to such other databases](#want-to-get-involved-in-the-development)).
- dbt-sugar is **really easy to configure** as long as your dbt `profiles.yml` is set up and you have a dbt project running all you need to do is **add a minimal set of config options**, which we call `syrup` (cos it's sweet) that looks like this:

```yaml
defaults:
  syrup: syrup_1
  target: dev
syrups:
  - name: syrup_1
    dbt_projects:
      - name: dbt_sugar_test
        path: "./tests/test_dbt_project/dbt_sugar_test"
        excluded_tables:
          - table_a
```

**Check out the [tool's documentation](https://bitpicky.gitbook.io/dbt-sugar/) to get you setup and configured.**

## Want to get involved in the development?

- Check out our [repository](https://github.com/bitpicky/dbt-sugar), we also tell you how you can start [contributing](https://github.com/bitpicky/dbt-sugar/blob/main/CONTRIBUTING.md)
- Report bugs or discuss features in a [GitHub issue](https://github.com/bitpicky/dbt-sugar/issues) or check our what's currently in the works in our [project boards](https://github.com/bitpicky/dbt-sugar/projects)
- Want to talk to a human, having some issues that you can't troubleshoot yourself, want to be part of the community? Join [bitpicky's Discord server](https://discord.com/invite/cQB49ejbCA)
