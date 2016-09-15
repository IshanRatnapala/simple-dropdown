define([
    'jquery',
    'underscore',
    'utility',
    'jquery/ui'
], function($, _, utility) {
    'use strict';

    /*
    * Use this if you have an element with content, a trigger element and you want them to function as a dropdown.
    *
    * Any element with the class 'close-dropdown' inside the content element will close the dropdown.
    * */

    $.widget('drgz.simpleDropdown', {
        options: {
            triggerSelector: null, /* Only if the trigger element is a child */
            contentSelector: '.content',
            searchGlobal: false, /* Search in the entire DOM for the content element */
            disableJQAnimation: false,
            slideSpeed: 250,
            closeOnBlur: true,
            activeClass: 'dropdown-active',
            afterCallback: false, /* Sends dropdown action and the content state in the parameters */
            debugMode: true
            /* TODO Add Disable for mobile, destroy functions */
        },
        
        _create: function () {
            var self = this;
            self._assignRoles();
            self._bindEvents();
        },

        _assignRoles: function () {
            var self = this;

            if (self.options.triggerSelector) {
                self.container = self.element;
                self.trigger = self.container.find(self.options.triggerSelector);
            } else {
                self.trigger = self.element;
            }

            /* Looking for the content element */
            self.content = self.element.find(self.options.contentSelector);
            if (self.options.searchGlobal) {
                self.content = $(self.options.contentSelector);
            } else if (!self.content.length) {
                self.content = self.element.siblings(self.options.contentSelector).length ?
                    self.element.siblings(self.options.contentSelector) :
                    self.element.siblings().find(self.options.contentSelector);
            }
        },

        _bindEvents: function () {
            var self = this;

            self.trigger.on('click', function (event) {
                event.preventDefault();
                self._dropDownAction('toggle');
            });

            self.content.find('.close-dropdown').on('click', function (event) {
                event.preventDefault();
                self._dropDownAction('close');
            });

            if (self.options.closeOnBlur) {
                $('body').on('click', function (event) {
                    if (!utility.contains(self.trigger, event.target, true) && !utility.contains(self.content, event.target, true)) {
                        self._dropDownAction('close');
                    }
                });
            }
        },

        _dropDownAction: function (action) { /* actions: open, close, toggle */
            var self = this;
            var animation;
            if (action === 'open') {
                action = 'addClass';
                animation = 'slideDown';
            } else if (action === 'close') {
                action = 'removeClass';
                animation = 'slideUp';
            } else if (action === 'toggle') {
                action = 'toggleClass';
                animation = 'slideToggle';
            }
            /* Do the given action */
            self.container && self.container[action](self.options.activeClass);
            self.trigger[action](self.options.activeClass);
            self.content[action](self.options.activeClass);
            if (!self.options.disableJQAnimation) {
                self.content[animation](self.options.slideSpeed);
            }

            self.options.afterCallback && self.options.afterCallback(action, self.content.is(':visible'));
        }
    });
    
    return $.drgz.simpleDropdown;
});
