---
published: false
title: Styling Visited Links with Radium and React
layout: post
---
Radium is a library for styling React components that I’ve spent a fair amount of development time on over the past few months. Radium uses inline styles exclusively, and there are some things you just can’t do without CSS. Styling :visited links is one of them. Luckily, Radium provides a <Style> component which makes this pretty easy to accomplish.

The <Style> component will render a <style> tag, and will prepend each selector with the specified scopeSelector. We use the name of the component as the class, but you could be extra careful and append a generated string to the class to be certain it won’t conflict.

```as
import {Component} from 'react';
import Radium, {Style} from 'radium';
 
@Radium
export default class ListOfLinks extends Component {
  render() {
    return (
      <div className="ListOfLinks">
        <Style
          scopeSelector=".ListOfLinks"
          rules={{
            a: {
              color: 'black'
            },
            'a:visited': {
              color: '#999'
            }
          }}
        />
        <ul>
          <li><a href="http://example1.com">Example 1</a></li>
          <li><a href="http://example2.com">Example 2</a></li>
          <li><a href="http://example3.com">Example 3</a></li>
          <li><a href="http://example4.com">Example 4</a></li>
        </ul>
      </div>
    );
  }
}
```

You’ll notice I also styled normal links, and that was just for convenience. If I style them inline, like <a href="http://example1.com" style={{color: 'blue'}}>Example 1</a>, I’d have to change the value for visited to #999 !important to make it override the inline style.

Remember, the usual caveats of selectors apply to <Style>, so any children of ListOfLinks that render anchor tags will also be affected. For this reason, you should only use <Style> on components that don’t render {this.props.children}.

Other uses for <Style>:

Styling user-generated HTML, like in a CMS
Styling the body and html tags (since scopeSelector is optional)