# Finishing Touches

This tutorial covers general concepts and structure for creating basic blocks.

## Additional Components

The block editor provides a [components package](/packages/components/README.md) which contains numerous prebuilt components you can use to build your block.

You can visually browse the components and what their implementation looks like using the Storybook tool published at [https://wordpress.github.io/gutenberg].

## Additional Tutorials

The **RichText component** allows for creating a richer input besides plain text, allowing for bold, italic, links, and other inline formating. See the [RichText Reference](/docs/designers-developers/developers/richtext.md) for documentation using this component.

The InspectorPanel (the settings on the right for a block) and Block Controls (toolbar controls) have a standard way to be implemented. See the [Block controls tutorial](/docs/designers-developers/developers/tutorials/block-tutorial/block-controls-toolbar-and-sidebar.md) for additional information.

The [Sidebar tutorial](/docs/designers-developers/developers/tutorials/sidebar-tutorial/plugin-sidebar-0.md) is a good resource on how to create a sidebar for your plugin.

Nested blocks, a block that contains additional blocks, is a common pattern used by various blocks such as Columns, Cover, and Social Links. The **InnerBlocks component** enables this functionality, see the [Using InnerBlocks documentation](/docs/designers-developers/developers/tutorials/block-tutorial/nested-blocks-inner-blocks.md).

## How did they do that

One of the best sources for information and reference is the Block Editor itself, all the core blocks are built the same way. A good way to learn how things are done is to find a core block code that does something close to what you are interested in and then using the same approach for your own block.

All core blocks source are in the [block library package on Github](https://github.com/WordPress/gutenberg/tree/master/packages/block-library/src).

[原文](https://github.com/WordPress/gutenberg/blob/master/docs/designers-developers/developers/tutorials/create-block/finishing.md)