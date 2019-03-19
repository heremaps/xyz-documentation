# Release notes for XYZ

### Material 3.x to 4.x

* Material for MkDocs 4.x finally fixes incorrect layout on Chinese systems.
  The fix includes a mandatory change of the base font-size from `10px` to
  `20px` which means all `rem` values needed to be updated. Within the theme,
  `px` to `rem` calculation is now encapsulated in a new function called
  `px2rem` which is part of the SASS code base.

* If you use Material with custom CSS that is based on `rem` values, note that
  those values must now be divided by 2. Now, `1.0rem` doesn't map to `10px`,
  but `20px`. To learn more about the problem and implications, please refer
  to [the issue][2] in which the problem was discovered and fixed.

  [2]: https://github.com/squidfunk/mkdocs-material/issues/911
  
### Studio and Viewer release 0.1.18

Today we’re excited to introduce a new experience for mobile users and updated basemaps!

TL;DR:
A new UX for mobile devices
Improved mobile UX for published projects
Updated basemaps
Studio dashboard view preference is now saved
Coming soon: improved data loading, account management dashboard


:sparkles: NEW :sparkles:
Now you can manage your projects on the go! We’re introducing a new Studio experience for mobile devices. You can view your project dashboard, adjust project settings, and easily view your published maps.


:hammer: IMPROVED :wrench:
We’ve also updated the mobile experience for published maps, making it easier to view feature cards alongside the legend panel that was introduced in our last release.
Our basemap styles have received a polish, and they’re looking spiffy. You’ll see the familiar color themes, but notice enhanced contrast for data visualization, as well as more detailed line work and road hierarchy across zooms.
As requested, your preference for list view or thumbnail view on the Studio dashboard is now saved.


:ant: FIXED :ant:
A few fixes were made for feature editing in Studio.


:crystal_ball: COMING SOON :crystal_ball:
Improved data loading, for a smoother experience when working with large datasets.
Account management dashboard, for checking up on your data usage.


Please send any questions or feedback our way, via Spark/Webex Teams, Slack or email. Thanks!

_____________________________________________________________________

### Studio and Viewer release 0.1.17
The XYZ Studio team is back with some new features that will help you tell even more compelling stories with your maps!

TL;DR:
You can now add a legend to your published projects!
 You can also add a title and description.
There is a new, easy way to turn cards on or off for published projects.
Coming soon: improved experience on mobile devices.

:sparkles: NEW :sparkles:

At long last, legends! You can now add a legend to your published project, which will show the symbol and name for each style group on your map.  Preview your legend in Publish Settings, and if you like what you see, you can add it right then and there to your project. We hope you’re as excited about this as we are.
But wait, there’s more: In Publish Settings, you can also choose to add your project’s title and description to the published version.
You can now easily turn off cards completely for your published project.
If the Cards option is not selected in Publish Settings, feature information cards will not be shown at all in your published project.
If you select the Cards option, they will show on click, with an option to view on hover, in your published project.
You probably didn’t need that explanation of how checkboxes work.

:hammer: IMPROVED :wrench:

You can now create style groups based on >= (greater than or equal to) or <= (less than or equal to) conditions. This improvement is guaranteed to make your new style groups >=  all your previous styles.
Style group symbols in the Studio side panel now more closely match the symbols on your map, rather than simply indicating feature type and color. Fancy!
Other minor UI improvements

:ant: FIXED :ant:

An error that would occasionally be shown during the file upload process, despite a successful upload, has been fixed.
Style groups based on Feature ID are now rendering correctly on published maps.
Other minor bug fixes


:crystal_ball: COMING SOON :crystal_ball:

In our next release, we’ll be introducing an improved mobile experience. You’ll be able to log into Studio, view your projects, and access Publish Settings for your projects.


Please send any questions or feedback our way, via Spark/Webex Teams, Slack or email. Thanks!

_____________________________________________________________________

### Studio and Viewer release 0.1.16
We are ringing in the new year with some exciting new features in Studio! We’re also trying out a new format for release notes, with the hope that it will be easier to understand the improvements and fixes in each release, and how they impact your use of Studio. Let us know what you think!

TL;DR:
Create style groups to style data according to property values.
Drag and drop a file to the map to begin uploading the file for use in that project.
Coming soon: options to display the project title, description, and legend in published maps.

:sparkles: NEW :sparkles:

You now have the ability to style the data on your map according to feature property values!

Where you once could adjust the style of the features on your map as a single group, there is now an option to add a new point/line/polygon style. This button opens a popup where you can define a style group.

You can also create a new group by clicking on a feature on the map, and then clicking the paint roller icon next to a property value in the sidebar. This will create a new group based on that property value, and you can always customize it further.
:world_map: Example: Say you are working with a building dataset, and there is a “year_built” property. You could create a style group for features where “year_built”  is greater than 1990. All features in the dataset where the building was built after 1990 would be included in the style group, and you can easily highlight these features on your map.

You can add on to rules using the “and” button to further filter the features included in the group.
:world_map: Example: Using the building dataset example from above, let’s say there is also a property called “frame_material”. You could also click the “and” button to add “frame_material equals wood”, to include only wood-frame buildings built after 1990 in the group.

To adjust or delete a group, click the gear icon next to the group name in the feature styling interface.

When you have multiple style groups within a feature type (points, lines or polygons), you can adjust the order of appearance on the map by dragging the rule group up or down in the styling interface.

You can now drag and drop a file to the map in a project to begin uploading the file for use in that (and other) projects.

Dragging a dropping a file to the map will open the “Add data” interface, and once your file is done uploading, it will be auto-selected to add to your project—you just need to click the “Add 1 dataset” button.

:crystal_ball: COMING SOON :crystal_ball:

We will soon be releasing new functionality that will provide the option to display a project’s title, description, and legend information in the published map. It will be a great way to help you tell a story through your map, and will show off all the cool new style groups you’ve created.


Please send any questions or feedback my way, via Slack, Spark/Webex Teams, or email. Thanks!
