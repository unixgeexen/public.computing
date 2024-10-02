Jira
============
* Based on Pluralsight "JIRA Fundamentals" Jan-Erik Sanberg `URL <https://app.pluralsight.com/course-player?clipId=2fe34194-7fe2-4cfe-9f84-01d2aaf44386>`_


Introduction to JIRA
********************
* Everything is in a project
* there are different issue types e.g. bug, story ...
* Generally view your project through a dashboard view

Planning JIRA Setup
*******************
* Server, Data Centre or Cloud

Installing and Configuring JIRA
********************************

Project Setup
*************
* Intro and basics
* Basic setup documentation
   * Create new project
   * Select project type
      * Software Development - Scrum, Basic or Kanban
      * Project Mgmt
      * Process Mgmt
      * Task Mgmt
   * View is via a board - cards (work) moveable between columns (status)
   * Create sprints
   * Add users
   * Create groups
   * Components Tab - Add them
      * Subsection of a project
* JIRA structure
   * Artifact
   * Configuration
   * Scheme - map configuration to issue type
   * Issue Types - Software Projects
      * Epic - big story, significant deliverable, new feature
      * Bug - impairment to function of Product
      * Story - user story, smallest unit of work to be done
      * Task - work to be done
      * Subtask - subcomponent of Task - work to be done
* Custom Fields Demonstration
   * Administration > Issues > Custom Field
   * Add to screen(s) required
* Custom Issue Type Demonstration
   * Administration > Issues > Issue Type > Create
   * Administration > Issues > Scheme # also required
   * Administration > Issues > Screen # also required
   * Administration > Issues > ScreenScheme # also required
* JIRA Workflow
   * Workflow Scheme - steps + transitions
   * steps have status
   * transitions - properties, triggers, conditions, validators, post functions
* Workflow Demonstration
   * New status - ready for QA
   * Issue Attributes > Statuses
   * Workflows - can be active or inactive - then publish
* Summary

JIRA Query Language (JQL)
*************************
* Intro and JQL basics - filtering Issues
   * field operator value
   * search box
   * issues > search # basic search
   * go into list view
   * select advanced next to search button
* Advanced Searching
   * keywords - AND, OR, NOT, EMPTY, NULL, ORDER BY
   * operators - match (=, !=, > etc) content (IN, NOT IN,CONTAINS ~, DOES NOT CONTAIN !~ )
   * advanced operators - current (IS, IS NOT), Historic (WAS, WAS IN, WAS NOT IN, WAS NOT, CHANGED) - only selected fields where historic data is stored
   * time - updated > "2019-01-01", updated > -2w, updated > -"4w 3d"
   * functions - time, login, end of month
   * description ~ web web* w??
   * filters - saved query (Save As button) - filter="BlueRobot Bugs"
   * columns button used to add/remove columns
   * Issues > Manage Filters
* Reporting
   * options - built-in, templates usually add reports, Marketplace, Custom
   * BYO reports - Atlassian Plugin SDK, Code, Deploy
   * Reports icon on LHS
* Dashboards
   * add pre-built gadgets usually based on filters
   * useful dashboards - actionable information, me first, important issues, team relevant, product, integrations
   * dashboards > manage dashboards > create
   * edit dashboard > edit layout > add gadget
   * share dashboard
   * set up wallboards slideshow - move through a set of dashboards
* Summary

Administering and Extending
***************************
* Routine followup - monitor users and remove if necessary, storage usage - remove unnecessary information, DB - optimise indexes
* Effective Teams
   * Workflows - ensure they keep up with what you need
   * 3rd Party Tools - check for new Tools
   * Projects - review more efficient templates
* Extending JIRA
    * Add-ons
    * Listeners - event response
    * Services - custom code, pull data from external systems
    * Webhooks - user defined http callback
       * requestb.in - for debugging
       * JIRA > Administration > Webhooks - hook back to requestb.in
       * Activate in JIRA and refresh to see data on requestb.in
*  Evolving with JIRA
    * JIRA Service Desk - customer Development
       * Administration > Discover Application tab
       * Project Templates
       * JIRA > Projects > Customer Portal
    * Confluence - deep integration with JIRA, documentation
       * Confluence space is similar to JIRA project
       * From JIRA - More > Link > Confluence
    * Statuspage - information when JIRA site is down

Summary and Way Forward
***********************
* Avoid
    * Labels - may encourage poor practices - is there any other way to handle my requirements
    * Interactions - ensure you still interact with humans
    * Multiple truths - don't let this happen - single source of truth
* Way Forward
    * integrate
    * learn
    * update JIRA constantly