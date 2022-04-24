# CMS App Example

Just a small app which creates 2 blocks in the shopware administration.

## Expectation vs Reality

The documentation on [add-custom-cms-blocks](https://developer.shopware.com/docs/guides/plugins/apps/content/cms/add-custom-cms-blocks#defining-blocks) is missing some piece of information which causes unexpected behaviour when creating a CMS App.

### Expectation

My expectation when creating a CMS app and looking at the [CMS-Reference](https://developer.shopware.com/docs/resources/references/app-reference/cms-reference) is that the slots which are written in the cms.xml are rendered by their order.

Looking at the docs the slot names always are: left, middle, right. Now that works for 3 elements if you always use these slot names but it gets kind of tricky when you add more than 3 elements.

A simple example: [The first blocks code:](/Resources/cms.xml)

```
<block>
            <name>ninja-example-block-1</name>
            <category>text</category>
            <label>YouTube - Text (normal)</label>
            <label lang="de-DE">YouTube - Text (normal)</label>

            <!-- Example with normal slot names -->
            <!-- Expectation: YouTube-Video on the left side, text element on the right side. -->
            <!-- Result: Text left and YouTube Video right -->
            <slots>
                 <slot name="video" type="youtube-video">
                    <config>
                        <config-value name="display-mode" source="static" value="contain"/>
                    </config>
                </slot>

                <slot name="text" type="text">
                    <config>
                        <config-value name="vertical-align" source="static" value="top"/>
                    </config>
                </slot>
            </slots>

            <default-config>
                <margin-top>20px</margin-top>
                <margin-right>20px</margin-right>
                <margin-bottom>20px</margin-bottom>
                <margin-left>20px</margin-left>
                <sizing-mode>boxed</sizing-mode>
            </default-config>
        </block>
```

The slot names here are choosed differently. First slot is called video and the second slot text. My expectation is that I get the slot that I defined first in this file will be rendered first.

The result: ![First Block](/Resources/images/firstBlock.PNG)

As you can see the blocks are rendered in the wrong order. Why?

### Reality

I played a lot around with the CMS App and tried to figure out what went wrong (it's hard to see whats wrong when all your placed elements in a block are the same). Now I just created the exact same block but with different slot names.
[The second blocks code:](/Resources/cms.xml)

```
        <!-- Same block but alphabetic -->
        <block>
            <name>ninja-example-block-2</name>
            <category>text</category>
            <label>YouTube - Text (alphabetic)</label>
            <label lang="de-DE">YouTube - Text (alphabetic)</label>

            <!-- Example with normal slot names -->
            <!-- Expectation: YouTube-Video on the left side, text element on the right side. -->
            <!-- Result: as Expectation -->
            <slots>
                 <slot name="a" type="youtube-video">
                    <config>
                        <config-value name="display-mode" source="static" value="contain"/>
                    </config>
                </slot>

                <slot name="b" type="text">
                    <config>
                        <config-value name="vertical-align" source="static" value="top"/>
                    </config>
                </slot>
            </slots>

            <default-config>
                <margin-top>20px</margin-top>
                <margin-right>20px</margin-right>
                <margin-bottom>20px</margin-bottom>
                <margin-left>20px</margin-left>
                <sizing-mode>boxed</sizing-mode>
            </default-config>
        </block>
```

The slot names here are "a" and "b". The result: ![Second Block](/Resources/images/secondBlock.PNG)

The elements are rendered alphabetically! There is no information for this on the docs and I'm not sure if this makes sense since it can get very complex when you add a couple of elements in a block. Quick-fix probably would be to just add this information to the docs.

## The problem

This example placed a YouTube-Video and a Text-Element next to each other. If you don't know that they are sorted alphabetically and you only put 2 text elements next to each other you will not realize that the elements are maybe switched. If you then add different texts to the elements you will be confused why you have a different output in the frontend.

I also noticed this:<br/>
![admin Blocks](/Resources/images/adminBlocks.PNG)<br/>
The elements are here also rendered alphabetically from their block-name. If you create different Layouts and the block names would be "layout-one", "layout-two", "layout-three" the output here also would be not as expected which can be confusing.

### Solution

Add information to the docs (quick and easy) or maybe change the order of the rendering
