namespace MyTested.AspNetCore.Mvc
{
    using Builders.Contracts.ActionResults.Base;
    using Builders.Contracts.Base;
    using Internal.Contracts.ActionResults;
    using Microsoft.AspNetCore.Mvc;
    using Utilities.Validators;

    /// <summary>
    /// Contains extension methods for <see cref="IBaseTestBuilderWithUrlHelperResult{TUrlHelperResultTestBuilder}"/>.
    /// </summary>
    public static class BaseTestBuilderWithUrlHelperResultExtensions
    {
        /// <summary>
        /// Tests whether the <see cref="ActionResult"/>
        /// has the same <see cref="IUrlHelper"/> as the provided one.
        /// </summary>
        /// <param name="baseTestBuilderWithUrlHelper">
        /// Instance of <see cref="IBaseTestBuilderWithUrlHelperResult{TUrlHelperResultTestBuilder}"/> type.
        /// </param>
        /// <param name="urlHelper">URL helper of type <see cref="IUrlHelper"/>.</param>
        /// <returns>The same URL helper <see cref="ActionResult"/> test builder.</returns>
        public static TUrlHelperResultTestBuilder WithUrlHelper<TUrlHelperResultTestBuilder>(
            this IBaseTestBuilderWithUrlHelperResult<TUrlHelperResultTestBuilder> baseTestBuilderWithUrlHelper,
            IUrlHelper urlHelper)
            where TUrlHelperResultTestBuilder : IBaseTestBuilderWithActionResult
        {
            var actualBuilder = GetActualBuilder(baseTestBuilderWithUrlHelper);

            RouteActionResultValidator.ValidateUrlHelper(
                actualBuilder.TestContext.MethodResult,
                urlHelper,
                actualBuilder.ThrowNewFailedValidationException);

            return actualBuilder.ResultTestBuilder;
        }

        /// <summary>
        /// Tests whether the <see cref="ActionResult"/>
        /// has the same <see cref="IUrlHelper"/> type as the provided one.
        /// </summary>
        /// <param name="baseTestBuilderWithUrlHelper">
        /// Instance of <see cref="IBaseTestBuilderWithUrlHelperResult{TUrlHelperResultTestBuilder}"/> type.
        /// </param>
        /// <returns>The same URL helper <see cref="ActionResult"/> test builder.</returns>
        public static TUrlHelperResultTestBuilder WithUrlHelperOfType<TUrlHelperResultTestBuilder, TUrlHelper>(
            this IBaseTestBuilderWithUrlHelperResult<TUrlHelperResultTestBuilder> baseTestBuilderWithUrlHelper)
            where TUrlHelper : IUrlHelper
            where TUrlHelperResultTestBuilder : IBaseTestBuilderWithActionResult
        {
            var actualBuilder = GetActualBuilder(baseTestBuilderWithUrlHelper);

            RouteActionResultValidator.ValidateUrlHelperOfType<TUrlHelper>(
                actualBuilder.TestContext.MethodResult,
                actualBuilder.ThrowNewFailedValidationException);

            return actualBuilder.ResultTestBuilder;
        }

        private static IBaseTestBuilderWithUrlHelperResultInternal<TUrlHelperResultTestBuilder>
            GetActualBuilder<TUrlHelperResultTestBuilder>(
                IBaseTestBuilderWithUrlHelperResult<TUrlHelperResultTestBuilder> baseTestBuilderWithUrlHelperResult)
            where TUrlHelperResultTestBuilder : IBaseTestBuilderWithActionResult
            => (IBaseTestBuilderWithUrlHelperResultInternal<TUrlHelperResultTestBuilder>)baseTestBuilderWithUrlHelperResult;
            
            public static class ContentTestBuilderExtensions
    {
        /// <summary>
        /// Tests whether the <see cref="Microsoft.AspNetCore.Mvc.ContentResult"/>
        /// has the same content as the provided one.
        /// </summary>
        /// <param name="contentTestBuilder">
        /// Instance of <see cref="IContentTestBuilder"/> type.
        /// </param>
        /// <param name="content">Expected content as string.</param>
        /// <returns>The same <see cref="IAndContentTestBuilder"/>.</returns>
        public static IAndContentTestBuilder WithContent(
            this IContentTestBuilder contentTestBuilder,
            string content)
        {
            var actualBuilder = (ContentTestBuilder)contentTestBuilder;

            var actualContent = actualBuilder.ActionResult.Content;

            if (content != actualContent)
            {
                throw ContentResultAssertionException.ForEquality(
                    actualBuilder.TestContext.ExceptionMessagePrefix,
                    content,
                    actualContent);
            }

            return actualBuilder;
        }

        /// <summary>
        /// Tests whether the <see cref="Microsoft.AspNetCore.Mvc.ContentResult"/>
        /// has content passing the given assertions.
        /// </summary>
        /// <param name="contentTestBuilder">
        /// Instance of <see cref="IContentTestBuilder"/> type.
        /// </param>
        /// <param name="assertions">Action containing all assertions for the content.</param>
        /// <returns>The same <see cref="IAndContentTestBuilder"/>.</returns>
        public static IAndContentTestBuilder WithContent(
            this IContentTestBuilder contentTestBuilder,
            Action<string> assertions)
        {
            var actualBuilder = (ContentTestBuilder)contentTestBuilder;

            var actualContent = actualBuilder.ActionResult.Content;

            assertions(actualContent);

            return actualBuilder;
        }

        /// <summary>
        /// Tests whether the <see cref="Microsoft.AspNetCore.Mvc.ContentResult"/>
        /// has content passing the given predicate.
        /// </summary>
        /// <param name="contentTestBuilder">
        /// Instance of <see cref="IContentTestBuilder"/> type.
        /// </param>
        /// <param name="predicate">Predicate testing the content.</param>
        /// <returns>The same <see cref="IAndContentTestBuilder"/>.</returns>
        public static IAndContentTestBuilder WithContent(
            this IContentTestBuilder contentTestBuilder,
            Func<string, bool> predicate)
        {
            var actualBuilder = (ContentTestBuilder)contentTestBuilder;

            var actualContent = actualBuilder.ActionResult.Content;

            if (!predicate(actualContent))
            {
                throw ContentResultAssertionException.ForPredicate(
                    actualBuilder.TestContext.ExceptionMessagePrefix,
                    actualContent);
            }

            return actualBuilder;
            
            
    }
}
