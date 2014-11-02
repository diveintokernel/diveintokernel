---
layout: post
title:  "How To Wirte Post!"
date:   2014-11-01 02:38:35
categories: guide
---

This sites offers powerful support for code snippets:

* ruby

The source is: 

{% highlight text %}
{% raw %}{% highlight ruby %}{% endraw %}
def print_hi(name)
  puts "Hi, #{name}"
end
print_hi('Tom')
#=> prints 'Hi, Tom' to STDOUT.
{% raw %}{% endhighlight %}{% endraw %}
{% endhighlight %}

The result is:

{% highlight ruby %}
def print_hi(name)
  puts "Hi, #{name}"
end
print_hi('Tom')
#=> prints 'Hi, Tom' to STDOUT.
{% endhighlight %}

* c

The source is:
{% highlight text %}
{% raw %}{% highlight c %}{% endraw %}
static void hisi_restart(enum reboot_mode mode, const char *cmd)
{
        writel_relaxed(0xdeadbeef, base + reboot_offset);

        while (1)
                cpu_do_idle();
}

static int hisi_reboot_probe(struct platform_device *pdev)
{
        struct device_node *np = pdev->dev.of_node;

        base = of_iomap(np, 0);
        if (!base) {
                WARN(1, "failed to map base address");
                return -ENODEV;
        }

        if (of_property_read_u32(np, "reboot-offset", &reboot_offset) < 0) {
                pr_err("failed to find reboot-offset property\n");
                return -EINVAL;
        }

        arm_pm_restart = hisi_restart;

        return 0;
}
{% raw %}{% endhighlight %}{% endraw %}
{% endhighlight %}

The result is:

{% highlight c %}
static void hisi_restart(enum reboot_mode mode, const char *cmd)
{
        writel_relaxed(0xdeadbeef, base + reboot_offset);

        while (1)
                cpu_do_idle();
}

static int hisi_reboot_probe(struct platform_device *pdev)
{
        struct device_node *np = pdev->dev.of_node;

        base = of_iomap(np, 0);
        if (!base) {
                WARN(1, "failed to map base address");
                return -ENODEV;
        }

        if (of_property_read_u32(np, "reboot-offset", &reboot_offset) < 0) {
                pr_err("failed to find reboot-offset property\n");
                return -EINVAL;
        }

        arm_pm_restart = hisi_restart;

        return 0;
}

{% endhighlight %}

* assembly


The source is:

{% highlight text %}
{% raw %}{% highlight asm %}{% endraw %}
/* Determine validity of the r2 atags pointer.  The heuristic requires
 * that the pointer be aligned, in the first 16k of physical RAM and
 * that the ATAG_CORE marker is first and present.  If CONFIG_OF_FLATTREE
 * is selected, then it will also accept a dtb pointer.  Future revisions
 * of this function may be more lenient with the physical address and
 * may also be able to move the ATAGS block if necessary.
 *
 * Returns:
 *  r2 either valid atags pointer, valid dtb pointer, or zero
 *  r5, r6 corrupted
 */
__vet_atags:
        tst     r2, #0x3                        @ aligned?
        bne     1f  

        ldr     r5, [r2, #0] 
#ifdef CONFIG_OF_FLATTREE
        ldr     r6, =OF_DT_MAGIC                @ is it a DTB?
        cmp     r5, r6
        beq     2f  
#endif
        cmp     r5, #ATAG_CORE_SIZE             @ is first tag ATAG_CORE?
        cmpne   r5, #ATAG_CORE_SIZE_EMPTY
        bne     1f  
        ldr     r5, [r2, #4] 
        ldr     r6, =ATAG_CORE
        cmp     r5, r6
        bne     1f  

2:      ret     lr                              @ atag/dtb pointer is ok

1:      mov     r2, #0
        ret     lr  
ENDPROC(__vet_atags)

{% raw %}{% endhighlight %}{% endraw %}
{% endhighlight %}

The result is:

{% highlight asm %}
/* Determine validity of the r2 atags pointer.  The heuristic requires
 * that the pointer be aligned, in the first 16k of physical RAM and
 * that the ATAG_CORE marker is first and present.  If CONFIG_OF_FLATTREE
 * is selected, then it will also accept a dtb pointer.  Future revisions
 * of this function may be more lenient with the physical address and
 * may also be able to move the ATAGS block if necessary.
 *
 * Returns:
 *  r2 either valid atags pointer, valid dtb pointer, or zero
 *  r5, r6 corrupted
 */
__vet_atags:
        tst     r2, #0x3                        @ aligned?
        bne     1f  

        ldr     r5, [r2, #0] 
#ifdef CONFIG_OF_FLATTREE
        ldr     r6, =OF_DT_MAGIC                @ is it a DTB?
        cmp     r5, r6
        beq     2f  
#endif
        cmp     r5, #ATAG_CORE_SIZE             @ is first tag ATAG_CORE?
        cmpne   r5, #ATAG_CORE_SIZE_EMPTY
        bne     1f  
        ldr     r5, [r2, #4] 
        ldr     r6, =ATAG_CORE
        cmp     r5, r6
        bne     1f  

2:      ret     lr                              @ atag/dtb pointer is ok

1:      mov     r2, #0
        ret     lr  
ENDPROC(__vet_atags)
{% endhighlight %}
