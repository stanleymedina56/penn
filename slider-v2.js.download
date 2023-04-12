/**
 * Slider v2 JS implementation
 * 
 */

var psu_outreach_marketing = psu_outreach_marketing || { };

jQuery( function ( $ ) {

    var KEYCODE_ENTER = 13;
    var KEYCODE_LEFT_ARROW = 37;
    var KEYCODE_RIGHT_ARROW = 39;

    /**
     * Discretely applies all of the javascript events required to drive the provided slider.
     * 
     * @param {jquery select list} container
     * 
     */
    psu_outreach_marketing.slider_v2 = function ( container ) {
        
        // The currently selected slide
        var currentPage = container.find( ".pager li.page" ).first();

        /**
         * Retrieves the previous slide (wrapping to the last slide if no previous slide exists).
         * 
         * @returns {object} jquery wrapped set
         */
        var getPreviousSlide = function () {
            var prev = currentPage.prev( ".pager li.page" );
            if ( !prev.length ) {
                prev = container.find( ".pager li.page" ).last();
            }
            return container.find( ".hero-node-" + prev.data( "hero-id" ) );
        };

        /**
         * Retrieves the next slide (wrapping to the first slide if no next slide exists).
         * 
         * @returns {object} jquery wrapped set
         */
        var getNextSlide = function () {
            var next = currentPage.next( ".pager li.page" );
            if ( !next.length ) {
                next = container.find( ".pager li.page" ).first();
            }
            return container.find( ".hero-node-" + next.data( "hero-id" ) );
        };

        // Apply keyboard and click events to the individual pager nodes
        var nodes = container.find( ".pager li.page" );

        // If the enter key is used, retain focus.
        // If the arrow keys are used, move focus to the next (or previous) node
        nodes.keydown( function ( e ) {
            switch ( e.which ) {
                case KEYCODE_ENTER:
                    e.preventDefault();
                    // The click handler will blur $( this ), so re-focus directly after.
                    $( this ).trigger( "click" ).focus();
                    break;
                case KEYCODE_LEFT_ARROW:
                    container.find( ".pager li.prev" ).trigger( "click" );
                    currentPage.find( 'a' ).focus();
                    break;
                case KEYCODE_RIGHT_ARROW:
                    container.find( ".pager li.next" ).trigger( "click" );
                    currentPage.find( 'a' ).focus();
                    break;
            }
        } );


        nodes.click( function ( e ) {
            
            e.preventDefault();

            // Remove styles + classes from the previously selected pager node
            currentPage.find( "i" ).removeClass( "fa-circle" ).addClass( "fa-circle-o" );

            // Update the aria-label (remove "Current Slide:" from the label)
            currentPage.find( "a" ).attr( "aria-label", "View Slide: " + $( ".current-hero" ).find( ".title" ).text() );

            // Remove active class from the previously selected slide
            container.find( ".current-hero" ).removeClass( "current-hero" );


            // Change the current page
            currentPage = $( this );


            // Add styles + classes to the newly selected slide
            container.find( ".hero-node-" + currentPage.data( "hero-id" ) ).addClass( "current-hero" );
            $( this ).find( "i" ).removeClass( "fa-circle-o" ).addClass( "fa-circle" );

            // Update the aria-label for the current pager node
            $( this ).find( "a" ).attr( "aria-label", "Current Slide: " + $( ".current-hero" ).find( ".title" ).text() );

            // Update the aria-label for the previous / next chevrons
            container.find( ".pager li.prev a" ).attr( "aria-label", "Previous Slide: " + getPreviousSlide().find( ".title" ).text() );
            container.find( ".pager li.next a" ).attr( "aria-label", "Next Slide: " + getNextSlide().find( ".title" ).text() );

            // Blur the focus (clicks shouldn't retain focus)
            currentPage.blur();
        } );

        // Apply the keyboard and click events to the previous chevron
        var previous = container.find( ".pager li.prev" );

        previous.keydown( function ( e ) {
            if ( e.which === KEYCODE_ENTER ) {
                e.preventDefault();
                $( this ).trigger( "click" ).focus();
            }
        } );

        previous.click( function ( e ) {
            e.preventDefault();
            var prev = currentPage.prev( ".pager li.page" );
            if ( !prev.length ) {
                prev = container.find( ".pager li.page" ).last();
            }
            prev.trigger( "click" );
            $( this ).blur();
        } );

        // Apply the keyboard and click events to the next chevron
        var next = container.find( ".pager li.next" );

        next.keydown( function ( e ) {
            if ( e.which === KEYCODE_ENTER ) {
                e.preventDefault();
                $( this ).trigger( "click" ).focus();
            }
        } );

        next.click( function ( e ) {
            e.preventDefault();
            var next = currentPage.next( ".pager li.page" );
            if ( !next.length ) {
                next = container.find( ".pager li.page" ).first();
            }
            next.trigger( "click" );
            $( this ).blur();
        } );

        /**
         * Workaround for the lack of object-fit support for IE / Edge
         * 
         * If the image is taller than it is wide, change the constraint dimension
         */
        container.find( ".image-wrapper img" ).each(function(index, value) {
            var img = $( value );
            if( img.height() / img.width() < .5) {
                img.css( {
                    height: "100%",
                    width: "auto",
                    maxWidth: "none"
                } );
            }
        });

        /**
         * IOS Hack:
         * 
         * The height messes up on orientation change on non-emulated IOS devices.  BrowserStack 
         * devices will not exhibit the behavior.  It has to be a real device.
         * 
         */
        if ( navigator.userAgent.match( /\((iPod|iPhone|iPad)/ ) ) {
            $( window ).on( "orientationchange", function ( ) {
                var vw = $( window ).width();
                if ( vw < 1024 ) {
                    if ( vw < 768 ) {
                        vw = vw / 2;
                    } else {
                        vw = vw / 2.12;
                    }
                } else {
                    vw = vw / 3.45;
                }

                container.find( ".slides figure .image-wrapper" ).css( "height", vw );
            } );
        }

        if ( $.mobile ) {

            // Apply swipe gestures if on a mobile device
            container.find( ".slides" ).on( "swipeleft", function () {
                container.find( ".pager li.next" ).trigger( "click" );
            } ).on( "swiperight", function () {
                container.find( ".pager li.prev" ).trigger( "click" );
            } );
        }
    };

    // Tell jquery mobile not to touch the DOM (otherwise it may add unwanted classes and elements)
    if ( $.mobile ) {
        $.mobile.autoInitializePage = false;
    }
} );
