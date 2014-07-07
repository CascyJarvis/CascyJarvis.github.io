iOS7中自定义UITextField的Placeholder属性
-------
通过重写UITextField中的__drawPlaceholderInRect__方法自定义Placeholder的属性，包括颜色，字体，及在UITextField中呈现的位置。   
1.  创建继承自UITextField的类CustomTextField  
2.  在CustomTextField中重写__drawPlaceholderInRect__方法，代码如下

``` 
- (void)drawPlaceholderInRect:(CGRect)rect
{
    UIColor *placeholderColor = [UIColor whiteColor];//设置颜色
    [placeholderColor setFill];
    
    CGRect placeholderRect = CGRectMake(rect.origin.x, (rect.size.height- self.font.pointSize)/2-2, rect.size.width, self.font.pointSize+1);//设置距离
    
    
    NSMutableParagraphStyle *style = [[NSMutableParagraphStyle alloc] init];
    style.lineBreakMode = NSLineBreakByTruncatingTail;
    style.alignment = self.textAlignment;
    NSDictionary *attr = [NSDictionary dictionaryWithObjectsAndKeys:style,NSParagraphStyleAttributeName, self.font, NSFontAttributeName, placeholderColor, NSForegroundColorAttributeName, nil];
    
    [self.placeholder drawInRect:placeholderRect withAttributes:attr];
}

```