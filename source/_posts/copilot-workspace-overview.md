---
title: GitHub Copilot Workspace Overview
date: 2024-11-6 11:24:00
tags:
---

I recently gained access to the GitHub Copilot Workspace technical preview, and I'm incredibly impressed. Copilot Workspace was created to help developers resolve bugs and add/remove features to their repositories, all within the browser. Having access to this piece of software as well as GitHub Codespaces, it makes it incredibly easy to do remote development.

Copilot Workspace is super flexible. You can give it pretty much any task like adding a new feature, organizing project files, or even getting rid of bugs from GitHub Issues. Copilot Workspace "brainstorms" solutions after receiving a task, and when you like what is had generated, it makes a plan and writes the code for you. This is very different from the traditional (or not so traditional) way of copying and pasting code into a chat interface online or having slightly more advanced code completions within your IDE via regular Copilot.

I have created a very small public project called _RetroCode Explorer_. It's a static site on GitHub Pages (like my blog) that lets you map out your project's file structure and add descriptions. It's already working, but I wanted to give Copilot a simple challenge: adding a faint, rotating SVG icon to the background.

I can start a new "task" through Copilot Workspace by going to the repository and clicking "Copilot" under the "Code" button.

Prompt: "Modify the `ContentArea` of my web application to add a very faint SVG icon that scales to the width of the container while keeping its aspect ratio. Have it rotate very slowly, 360 degrees."

![copilot-workspace-overview](images/copilot-workspace-overview-1.png)

The picture shown below is the "brainstorm" function of Copilot Workspace. It showed me what Copilot understood about my project and its overall plan. It knew where the `ContentArea` component was, its stylesheet, and that there wasn't an SVG already there. I noticed that it talks about the changes in past tense, even though it hasn't written any code yet, which is a little weird. But there are auto-generated questions and a custom input bar to help you refine the task if you'd like.

![[Pasted image 20241106102350.png]]

Hitting "Generate code" showed me a more detailed plan and the code changes, like a commit view. It changed the CSS to position and animate the SVG and added the SVG to the component. 

![copilot-workspace-overview](images/copilot-workspace-overview-2.png)

```css
/* I'm not sure what the comments mean – maybe an artifact with how Copilot was trained? */

.content-area .svg-icon { /* Pc631 */
    position: absolute; /* P4c07 */
    top: 0; /* P4c07 */
    left: 0; /* P4c07 */
    width: 100%; /* Pc631 */
    height: auto; /* Pc631 */
    opacity: 0.1; /* Pc631 */
    z-index: 0; /* P4c07 */
    animation: rotate 60s linear infinite; /* Pc631 */
}

@keyframes rotate { /* Pc631 */
    from {
        transform: rotate(0deg); /* Pc631 */
    }
    to {
        transform: rotate(360deg); /* Pc631 */
    }
}
```

The changes to the component's JSX file was pretty straightforward as well.

```jsx
import { ReactComponent as FaintIcon } from './faint-icon.svg'; // Pae40

return (
        <div className="content-area">
            <div className="content-wrapper"> {/* Pf1bb */}
                <FaintIcon className="svg-icon" /> {/* P4190 */}
                <textarea
                    value={content}
                    onChange={handleChange}
                    placeholder={selectedItem
                        ? `Editing: ${selectedItem.name}`
                        : "Select a file or folder to begin editing..."}
                    disabled={!selectedItem}
                />
            </div> {/* Pf1bb */}
        </div>
    );
```

What makes this so powerful is that you can see the changes live in Codespaces. It even creates a new feature branch automatically.

![copilot-workspace-overview](images/copilot-workspace-overview-3.png)

In this instance, it had created a feature branch called `add-svg-icon-e11`, which is super convenient for pull requests. You can see below that I am already in the new feature branch as Codespace opens.

![copilot-workspace-overview](images/copilot-workspace-overview-4.png)

I added the SVG file (I used the default React one), and it almost worked perfectly. The size and rotation were good, but the color was off, and it was creating scrollbars.

![copilot-workspace-overview](images/copilot-workspace-overview-5.png)

I used the "revise" feature provided by Copilot Workspace to fix the size and color. 

Prompt: "Make the icon a darker gray than the background of the `content-area` and constrain the icon to the size of `content-area` so scroll bars aren't created." It took about 15 seconds, and it looked much better! The icon now is being shown as intended (it is a little lighter than the background but I think I prefer that more).

![copilot-workspace-overview](images/copilot-workspace-overview-6.png)

Since I was happy with how it turned out, I deployed it to the official site and began exploring options with merging the changes onto the main branch. Copilot Workspaces gives me a couple of options to choose from, but since pull requests are automatically generated, I decided to just choose the first option.

![copilot-workspace-overview](images/copilot-workspace-overview-7.png)

The title and description were automatically generated. There was about 30 seconds in between me clicking the green pull request button and having the pull request ready to be reviewed in my repository.

![copilot-workspace-overview](images/copilot-workspace-overview-8.png)

You can see the final result by navigating to the official [RetroCode Explorer](https://www.gabecoatess.com/retrocode-explorer/)page. The [GitHub Repository](https://www.github.com/gabecoatess/retrocode-explorer/) is public as well in case you want to take a peak at the source code.

Copilot Workspace is awesome, but a few things could be better. As with all language models (I believe Copilot would be classified as a language model, I could be wrong), it depends on the training data it has access to. But overall, it's a huge time-saver and makes coding a lot more fun for smaller features. I would still prefer to create this by hand but I believe that will change with some time.

As a little side note, my application is somewhat mobile friendly. These changes worked well on my iPhone as a PWA.
![copilot-workspace-overview](images/copilot-workspace-overview-9.png)