<!--
# Block Controls: Block Toolbar and Settings Sidebar
 -->
# ブロックコントロール: ブロックツールバーと設定サイドバー

<!--
To simplify block customization and ensure a consistent experience for users, there are a number of built-in UI patterns to help generate the editor preview. Like with the `RichText` component covered in the previous chapter, the `wp.editor` global includes a few other common components to render editing interfaces. In this chapter, we'll explore toolbars and the block inspector.
 -->
ブロックのカスタマイズを簡素化し、ユーザーに一貫した体験を与えるため、エディターのプレビュー生成を支援する多数の組み込み UI パターンがあります。前のセクションで触れた `RichText` コンポーネント同様、グローバル `wp.editor` には、編集インターフェースをレンダーする、その他の共通コンポーネントがいくつか含まれます。このセクションでは、ツールバーとブロックインスペクターについて説明します。

<!--
## Block Toolbar
 -->
## ブロックツールバー

<!-- 
![Screenshot of the rich text toolbar applied to a Paragraph block inside the block editor](https://raw.githubusercontent.com/WordPress/gutenberg/HEAD/docs/assets/toolbar-text.png)
 -->
![ブロックエディター内部で段落ブロックに適用されたリッチテキストツールバー](https://raw.githubusercontent.com/WordPress/gutenberg/HEAD/docs/assets/toolbar-text.png)

<!-- 
When the user selects a block, a number of control buttons may be shown in a toolbar above the selected block. Some of these block-level controls are included automatically if the editor is able to transform the block to another type, or if the focused element is a RichText component.
 -->
ユーザーがブロックを選択するとブロックの上のツールバーに複数のコントロールボタンが表示されます。エディターがブロックを他のタイプに変換できる場合、またはフォーカスを得た要素が RichText コンポーネントの場合、ブロックレベルコントロールのいくつかは自動的に表示されます。

<!-- 
You can also customize the toolbar to include controls specific to your block type. If the return value of your block type's `edit` function includes a `BlockControls` element, those controls will be shown in the selected block's toolbar.
 -->
ツールバーをカスタマイズして、ブロックタイプ固有のコントロールを表示することもできます。ブロックタイプの `edit` 関数の戻り値に `BlockControls` を加えると、ブロックのツールバーにそのコントロールが表示されます。

```jsx
import { registerBlockType } from '@wordpress/blocks';

import {
	useBlockProps,
	RichText,
	AlignmentToolbar,
	BlockControls,
} from '@wordpress/block-editor';

registerBlockType( 'gutenberg-examples/example-04-controls-esnext', {
	apiVersion: 3,
	title: 'Example: Controls (esnext)',
	icon: 'universal-access-alt',
	category: 'design',
	attributes: {
		content: {
			type: 'string',
			source: 'html',
			selector: 'p',
		},
		alignment: {
			type: 'string',
			default: 'none',
		},
	},
	example: {
		attributes: {
			content: 'Hello World',
			alignment: 'right',
		},
	},
	edit: ( { attributes, setAttributes } ) => {
		const onChangeContent = ( newContent ) => {
			setAttributes( { content: newContent } );
		};

		const onChangeAlignment = ( newAlignment ) => {
			setAttributes( {
				alignment: newAlignment === undefined ? 'none' : newAlignment,
			} );
		};

		return (
			<div { ...useBlockProps() }>
				{
					<BlockControls>
						<AlignmentToolbar
							value={ attributes.alignment }
							onChange={ onChangeAlignment }
						/>
					</BlockControls>
				}
				<RichText
					className={ attributes.className }
					style={ { textAlign: attributes.alignment } }
					tagName="p"
					onChange={ onChangeContent }
					value={ attributes.content }
				/>
			</div>
		);
	},
	save: ( props ) => {
		const blockProps = useBlockProps.save();

		return (
			<div { ...blockProps }>
				<RichText.Content
					className={ `gutenberg-examples-align-${ props.attributes.alignment }` }
					tagName="p"
					value={ props.attributes.content }
				/>
			</div>
		);
	},
} );
```

<!--
Note that `BlockControls` is only visible when the block is currently selected and in visual editing mode. `BlockControls` are not shown when editing a block in HTML editing mode.
 -->
注意: `BlockControls` はブロックがエディターのビジュアルモードで選択されている場合のみ表示されます。HTML 編集モードでは表示されません。

<!--
## Settings Sidebar
 -->
## 設定サイドバー

<!--
![Screenshot of the inspector panel focused on the settings for a Paragraph block](https://raw.githubusercontent.com/WordPress/gutenberg/HEAD/docs/assets/inspector.png)

The Settings Sidebar is used to display less-often-used settings or settings that require more screen space. The Settings Sidebar should be used for **block-level settings only**.

If you have settings that affects only selected content inside a block (example: the "bold" setting for selected text inside a paragraph): **do not place it inside the Settings Sidebar**. The Settings Sidebar is displayed even when editing a block in HTML mode, so it should only contain block-level settings.

The Block Tab is shown in place of the Document Tab when a block is selected.

Similar to rendering a toolbar, if you include an `InspectorControls` element in the return value of your block type's `edit` function, those controls will be shown in the Settings Sidebar region.
 -->
![段落ブロックの設定でフォーカスのあるインスペクターパネル](https://raw.githubusercontent.com/WordPress/gutenberg/HEAD/docs/assets/inspector.png)

設定サイドバーはあまり使わない設定や大きなスペースが必要な設定で使用します。設定サイドバーは**ブロックレベル設定でのみ**使用してください。

ブロック内の選択したコンテンツでのみ有効な設定は (例: Paragraph 内の選択したテキストの「太字」設定)、**設定サイドバーの中に含めないでください**。設定サイドバーは HTML モードでブロックを編集する場合にも表示されるため、ブロックレベルの設定のみを含めてください。

ブロックタブはブロックが選択されるとドキュメントタブと同じ位置に表示されます。

ツールバーのレンダーと同様、ブロックタイプの `edit` 関数の戻り値に `InspectorControls` 要素を追加すると、それらのコントロールは設定サイドバー領域に表示されます。

<!--
The following example adds 2 color palettes to the sidebar, one for the text color and one for the background color.
 -->
次の例では、サイドバーに2色のパレットを追加します。1つはテキスト色、もう1つは背景色です。

```jsx
import { registerBlockType } from '@wordpress/blocks';
import { __ } from '@wordpress/i18n';
import { TextControl } from '@wordpress/components';

import {
	useBlockProps,
	ColorPalette,
	InspectorControls,
} from '@wordpress/block-editor';

registerBlockType( 'create-block/gutenpride', {
	apiVersion: 3,
	attributes: {
		message: {
			type: 'string',
			source: 'text',
			selector: 'div',
			default: '', // empty default
		},
		bg_color: { type: 'string', default: '#000000' },
		text_color: { type: 'string', default: '#ffffff' },
	},
	edit: ( { attributes, setAttributes } ) => {
		const onChangeBGColor = ( hexColor ) => {
			setAttributes( { bg_color: hexColor } );
		};

		const onChangeTextColor = ( hexColor ) => {
			setAttributes( { text_color: hexColor } );
		};

		return (
			<div { ...useBlockProps() }>
				<InspectorControls key="setting">
					<div id="gutenpride-controls">
						<fieldset>
							<legend className="blocks-base-control__label">
								{ __( 'Background color', 'gutenpride' ) }
							</legend>
							<ColorPalette // Element Tag for Gutenberg standard colour selector
								onChange={ onChangeBGColor } // onChange event callback
							/>
						</fieldset>
						<fieldset>
							<legend className="blocks-base-control__label">
								{ __( 'Text color', 'gutenpride' ) }
							</legend>
							<ColorPalette // Element Tag for Gutenberg standard colour selector
								onChange={ onChangeTextColor } // onChange event callback
							/>
						</fieldset>
					</div>
				</InspectorControls>
				<TextControl
					value={ attributes.message }
					onChange={ ( val ) => setAttributes( { message: val } ) }
					style={ {
						backgroundColor: attributes.bg_color,
						color: attributes.text_color,
					} }
				/>
			</div>
		);
	},
	save: ( { attributes } ) => {
		return (
			<div
				{ ...useBlockProps.save() }
				style={ {
					backgroundColor: attributes.bg_color,
					color: attributes.text_color,
				} }
			>
				{ attributes.message }
			</div>
		);
	},
} );
```

<!--
Block controls rendered in both the toolbar and sidebar will also be used when
multiple blocks of the same type are selected.
 -->
ツールバーとサイドバー内の両方でレンダーされるブロックコントロールは、同じタイプの複数のブロックが選択された際にも使用されます。

<!-- 
**Note :** In the example above, we added text and background color customization support to our block to demonstrate the use of `InspectorControls` to add custom controls to the sidebar. That said, for common customization settings including color, border, spacing customization and more, we will see on the [next chapter](/docs/how-to-guides/block-tutorial/block-supports-in-static-blocks.md) that you can rely on block supports to provide the same functionality in a more efficient way.
 -->
**注意:** 上の例では、`InspectorControls` のデモとして、ブロックのテキストや背景色のカスタマイズのサポートに、`InspectorControls` を使用してサイドバーにカスタムコントロールを追加しました。しかし、色やボーダー、スペースなどの一般的なカスタマイズ設定では、[次の章](https://ja.wordpress.org/team/handbook/block-editor/how-to-guides/block-tutorial/block-supports-in-static-blocks/)で見るように、より効率的に同じ機能を提供するブロックサポートを使用できます。

[原文](https://github.com/WordPress/gutenberg/blob/trunk/docs/how-to-guides/block-tutorial/block-controls-toolbar-and-sidebar.md)
