---
layout: post
title: "How to append a footer to all your emails in Rails"
date: 2013-04-20 17:50
comments: true
categories: 
  - rails
  - ruby
  - email
  - programming
  - code
---

In Germany and many other countries as a company you are required to add a legal footer to your emails.

Of course you could just add the legal snippet into the footer of each of your emails.

Or you can create a partial and include it.

But theres a better way.

## Writing the test

So let's start with a test.

{% codeblock email_signature_spec.rb lang:ruby %}
    require 'spec_helper'

    class FakeMailer < ActionMailer::Base
      def any_email
        mail(from: 'a@example.com', to: 'b@example.com', subject: "Hello", body: 'A body')
      end
    end

    describe 'Email Signature' do
      it 'appends a signature to every email' do
        signature_divider = "-- \n"

        email = FakeMailer.any_email.deliver
        email.body.to_s.should match(signature_divider)
      end
    end
{% endcodeblock %}

This fails of course. I did match on the signature divider which contains of 2 dashes, a space and a newline. This is a old convention from the Newsnet and can be automatically parsed by many email clients.

## The implementation

An elegant way of solving this is to implement an mail interceptor.

An interceptor catches mails that are send and can modify their content.

{% codeblock email_signature.rb lang:ruby %}
     
     class EmailSignature
      def self.delivering_email(message)
        message.body = String(message.body) + footer
      end

      def self.footer
        "-- \n" +  "Insert your footer here"
      end
    end
    Mail.register_interceptor(EmailSignature)

{% endcodeblock %}

The interceptor in our case appends the footer to the body of the message. It needs to be registered with 

    Mail.register_interceptor

## Conclusion

This is a very simple way to add a footer to all your text emails.

It does not take into account HTML emails and email's with multiple parts. (Though it's no black magic to modify the code.)
